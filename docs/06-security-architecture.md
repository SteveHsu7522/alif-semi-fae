# Security Architecture — Secure Enclave

Sources: https://alifsemi.com/press-release/alif-semiconductor-security-architecture/ , system-architecture whitepaper, and the "Alif Security Toolkit" (SETOOLS) support page.

## Secure Enclave Subsystem (SESS)
Every Ensemble/Balletto device has a dedicated, isolated Secure Enclave that provides:
- Secure key management and storage
- Secure boot with an **immutable Root-of-Trust** (SESS always boots from a fixed ROM image first)
- Runtime attestation via certificates
- Hardware cryptographic acceleration
- Secure debugging (gated by lifecycle state)
- Read-out protection
- Secure firmware updates
- Power management orchestration (aiPM, see `05-power-management-aiPM.md`)
- Full device lifecycle management

## Key management
- Each device generates and stores its own unique key pair **inside the Secure Enclave during manufacturing** — no external HSM key-injection step is required.
- This lets a device establish a trust chain from network → device without relying on a third party post-deployment (relevant for OTA update signing, mutual TLS, attestation flows).

## Secure boot
1. SESS boots first, always from the immutable ROM.
2. It validates system integrity (signature verification against the ATOC / image headers) before releasing any application core.
3. See `04-system-architecture.md` for the ATOC-driven boot sequence detail.

## Lifecycle states (LCS)
| LCS | Meaning |
|---|---|
| **Development** | Factory-default, unsecured — debug stub available if MRAM@0x0 is empty, debugger can attach freely |
| **Secure** | Activated once user/OEM keys are provisioned — enforces access control, restricts debug |
| **RMA** | Return-Material-Authorization state — for field returns without exposing customer IP/keys |

Moving a device between lifecycle states is a **one-way, provisioning-time decision** in normal use (Development → Secure) — this is a common point of customer confusion: make sure they understand a device transitioned to Secure cannot trivially go back to open debug access, so LCS transition should be part of their production provisioning plan, tested thoroughly in Development state first.

## Isolation model — Firewalls
- Goes beyond plain Armv8-M TrustZone (secure/non-secure world split): Alif's configurable firewalls let memory regions and peripherals be assigned per-core (exclusive or shared) based on bus master ID + security attribute.
- Applies uniformly across Cortex-M55, Cortex-A32, and even NPU (Ethos-U55) transactions — i.e. the NPU's memory accesses are also subject to firewall/TrustZone policy, not a backdoor around it.

## Standards alignment
Alif positions this architecture against **IEC 62443**, **IEC/ISO 27001**, and **ISA/SAE 21434** (automotive cybersecurity) — useful when a customer's compliance/security team asks "what does this chip give us for free" toward a certification effort. It reduces engineering burden but does not by itself constitute certification — the customer's system integration and process still needs to be assessed.

## Tooling
- **Alif Security Toolkit (SETOOLS)** — CLI tool for security configuration/provisioning/key management on Ensemble & Balletto devices. Current published version at time of writing: v1.110.00. User guide covers all SETOOLS commands.
- **SE Host Services API** — host-side API to talk to the Secure Enclave (e.g. from a provisioning fixture or from Linux userspace on APSS). Source available via "Alif SE Host Services API Archive."
- **Conductor / Offline Conductor** — GUI tool that also covers security configuration (alongside peripherals/clocks/pin-mux/power) — good starting point for a customer who wants a visual walkthrough before dropping into SETOOLS CLI.

## FAE talking points
- "Do I need an external secure element?" → No — that's the core Secure Enclave value prop; keys are generated/stored on-die at manufacturing.
- "Can I debug a device I've already locked down for production?" → Depends on LCS and what secure-debug authentication was provisioned; point them at the SETOOLS user guide's secure-debug-authentication section rather than guessing — get the exact current flow from the guide since this is exactly the kind of detail that changes across SETOOLS versions.
- Always confirm the customer is on a current SETOOLS version before troubleshooting a provisioning issue — version mismatches between SETOOLS, the CMSIS DFP, and on-chip ROM are a common source of "why won't this device provision" tickets.
