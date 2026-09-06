# System Architecture (applies to Ensemble & Ensemble Gen AI fusion parts)

Source: https://alifsemi.com/whitepaper/fusion-processors-system-architecture-introduction/ — this is the single most important architecture reference gathered; read it in full on alifsemi.com if you need more depth than below.

## The four subsystems

### RTSS-HP — High-Performance Real-Time Subsystem
- Cortex-M55 up to 400MHz
- 256KB I-TCM + 1MB D-TCM (tightly coupled memory — deterministic, single-cycle access)
- Ethos-U55 with 256 MACs/cycle attached
- Meant for the heavier real-time/DSP/ML workload of a pair

### RTSS-HE — High-Efficiency Real-Time Subsystem
- Cortex-M55 up to 160MHz, lower-leakage process/voltage point
- 256KB I-TCM + 256KB D-TCM
- Ethos-U55 with 128 MACs/cycle
- **Can operate standalone with minimal dependency on the rest of the chip** — this is the key fact for always-on/low-power designs: sensor polling, wake-word detection, etc. can run entirely in RTSS-HE while APSS and RTSS-HP stay powered off.

### APSS — Application Processing Subsystem
- Dual Cortex-A32, each with 32KB L1 cache, shared 512KB L2 cache
- Runs Linux (see `linux_alif`, `meta-alif`, `meta-alif-ensemble` repos)
- Present on E5/E7 and E4/E6/E8; absent on E1/E1C/E3

### SESS — Secure Enclave Subsystem
- Acts as the **system supervisor** — isolated from the main interconnect
- Provides cryptographic acceleration, secure boot services, lifecycle management
- Every subsystem's release-from-reset is gated by SESS (see Boot Flow below)
- Full security detail in `06-security-architecture.md`

## Memory architecture
- **Shared SRAM**: SRAM0/SRAM1 blocks, accessible across domains
- **TCM**: per-core tightly coupled instruction/data memory (see RTSS-HP/HE above)
- **Backup SRAM**: 4KB, powered by the VBAT rail — survives most power-down states, used for minimal state retention across deep sleep
- **MRAM**: on-chip non-volatile storage (magneto-resistive RAM), 16-byte sector granularity — this is Alif's flash-replacement technology; faster and lower-power than typical embedded flash
- **External expansion**: OctalSPI/HexSPI interfaces support XiP (execute-in-place) with AES-128 decryption on the fly (i.e. you can execute encrypted code straight from external octal-SPI flash)
- Fragmented SRAM across domains can be consolidated logically via MMU config (APSS side) and firewall-based address translation (RTSS side)

## Boot flow
1. On power-up or a wake event, **SESS boots first**, performs chip initialization and security configuration.
2. SESS reads the **ATOC (Application Table of Contents)** — a structure stored at the top of MRAM — which specifies *which cores boot, from what address, in what order, and with what security attributes*.
3. SESS then releases the application cores per the ATOC.
4. **Lifecycle-state-dependent fallback**: in `LCS=Development`, if there's no valid code at MRAM address 0x0, SESS loads a **debug stub** so a debugger can attach and interactively program the device — this is why a fresh/blank dev-kit part is still "alive" enough to flash over SWD/JTAG.

## Inter-processor communication
- **MHU (Message Handling Unit)**: interrupt-driven, bidirectional, works across power/clock domain boundaries. 8 MHU instances organized as 2 full-duplex channels; TrustZone-aware (secure and non-secure paths).
- **HWSEM (Hardware Semaphore)**: 16 atomic synchronization primitives for arbitrating shared resources between cores, with interrupt-on-release.

## Firewalls / isolation
- TrustZone-compliant, but Alif's firewall mechanism goes further than plain Armv8-M TrustZone: memory regions and peripherals can be assigned to specific cores (shared or exclusive) based on **master ID** and security attribute, independent of which core originally owns the bus.
- This is the basis for "assign this peripheral to core X only" designs — relevant when a customer partitions work across RTSS-HP/RTSS-HE/APSS and wants hardware-enforced isolation, not just software convention.

## Where this matters for FAE work
- **"Why does my RTSS-HE application still boot fine with the HP core disabled?"** → because HE is designed to run standalone; check the ATOC config to make sure HP isn't in the boot chain if it's meant to stay off.
- **"Can core A see memory owned by core B?"** → depends on firewall config, not just TrustZone NS/S state; check the Conductor tool's peripheral/memory assignment for the specific project, and the CMSIS DFP's firewall config headers.
- **"How do I pass data between the M55 and the A32 Linux core?"** → MHU (interrupt path) is the low-level primitive; most customers will want to use a higher-level scheme built on it (mailboxes, ring buffers) rather than talking to MHU directly — check whether the SDK/RTOS in use (Zephyr, ThreadX, FreeRTOS) already exposes an IPC abstraction.
