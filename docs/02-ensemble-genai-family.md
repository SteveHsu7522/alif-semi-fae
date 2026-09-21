# Ensemble Gen AI Family (E4, E6, E8) — "2nd generation"

Source: https://alifsemi.com/products/ensemblegenai/ and PR/whitepaper coverage of the Jan 2025 CES launch. **E8-specific confirmed specs below are from the official Alif E8 Datasheet v1.0**, publicly hosted (no alifsemi.com login needed) at https://www.mouser.com/catalog/specsheets/Alif_E8_Datasheet_v1.0.pdf — distributor sites (Mouser, and likely Digi-Key/Arrow) mirror Alif's datasheet PDFs even though alifsemi.com/support gates the same documents behind a login. Worth checking a distributor mirror before telling a customer "you need an account" for any Alif datasheet/reference manual.

## What's different from base Ensemble
Same general fusion-processor architecture as E5/E7, but adds a **third, dedicated NPU** — the **Ethos-U85** — specifically built to accelerate **transformer networks** (attention layers), which the Ethos-U55 is not efficient at. This is the enabling piece for running generative-AI-style models (small LLMs, transformer-based vision/audio models) locally at the edge without a cloud round-trip.

## Core mix (top-end E8)
- 2x Cortex-A32 @ 800MHz (application cores, Linux-capable)
- 2x Cortex-M55 @ 400MHz (real-time cores, RTSS-HP/HE)
- 2x Ethos-U55 @ 400MHz (250+ GOPS combined) — same microNPU as base Ensemble, attached to the M55 cores
- 1x Ethos-U85 @ 400MHz (204 GOPS) — the new transformer-capable NPU

## Memory
- 9.75MB on-die SRAM (with retention options)
- 5.5MB on-die MRAM
- 15MB+ total integrated memory
- Expandable via HexSPI and SD/eMMC

## Positioning
"Multimodal AI in endpoint products focused on vision, voice, text, and sensor fusion" — i.e. the target use case is a device that needs to *locally* fuse multiple modalities (camera + mic + sensors) and reason over them with a transformer-style model, still within an MCU power/cost envelope, not a full applications processor.

## FAE talking points
- If a customer asks "can I run a small LLM / transformer / diffusion model on Ensemble" → yes, but only on **E4/E6/E8** (Ethos-U85 present); base E1/E3/E5/E7 only have Ethos-U55, which is CNN/RNN-oriented, not transformer-optimized.
- The E8 DevKit is the flagship evaluation platform for this family (see `09-dev-kits-eval-boards.md`).
- Software-side, transformer model support flows through Arm's ML tooling (Vela compiler / TensorFlow Lite Micro / Ethos-U driver stack) — same general toolchain story as the U55 parts, just targeting the U85 backend. Check `alif_ml-embedded-evaluation-kit` repo and Arm's Ethos-U85 documentation for current op support before promising a specific model will run.

## E8 confirmed datasheet specs (v1.0)

**Note on RTSS-HP/HE memory**: the datasheet gives E8's RTSS-HP TCM as **1.25MB** and RTSS-HE TCM as **512KB** (single figures, not split I/D). This is larger than the generic family-wide RTSS-HP/HE figures in `04-system-architecture.md` (256KB I-TCM+1MB D-TCM for HP, 256KB+256KB for HE) — those generic figures came from the system-architecture whitepaper and may describe a different/earlier part or a floor spec across the family. **When a customer asks about TCM size on a specific part, use that part's own datasheet number, not the generic whitepaper figure.**

### Core clocks (E8)
- APSS: dual Cortex-A32, up to 800MHz, 512KB shared L2, 32KB L1 I+D cache each
- RTSS-HP: Cortex-M55 up to 400MHz, 1.25MB TCM (0-wait-state)
- RTSS-HE: Cortex-M55 up to 160MHz, 512KB TCM (0-wait-state)
- System interconnect: 400MHz, 64-bit and 128-bit AXI bus fabric

### MIPI / camera / display — the important constraint
**A single E8 supports either (1 camera + 1 display) or (2 cameras + 0 extra display), never 2 cameras + 2 displays at once:**
- **1x MIPI CSI-2, 2-lane, 2.5Gbps/lane** — dedicated camera RX port
- **1x MIPI DSI, 2-lane, 2.5Gbps/lane** — display TX port
- **Shared D-PHY trick**: the DSI PHY can be reconfigured into CSI-2 **slave mode** to become a *second* camera RX port ("interleaving two image streams from two separate image sensors") — but doing this consumes the DSI PHY, so the native DSI display output is no longer available in that configuration.
- ISP: up to 400MHz, 64-bit bus, max input/output resolution **2MP (1920×1080)**, frame rate 1–60 FPS configurable, auto white balance / auto exposure. JPEG encoder works alongside the ISP (MJPEG video clip capture).
- Graphics LCD Controller (CDC): 1x DPI (Display Parallel Interface), 24-bit RGB — a non-MIPI display alternative when you don't want to spend the DSI port.
- Camera Parallel Interface (CPI): up to 16-bit (8-bit low-power variant) — a non-MIPI camera alternative.
- 2D GPU on-chip for graphics acceleration (see D/AVE2D in `07-software-tools-ides.md`).

**Design implication**: a "2 MIPI camera + 2 MIPI display" requirement cannot be met by one E8. Two E8s (each running the clean 1×CSI-2 + 1×DSI config) is the straightforward way to do it — confirmed as the right call when a customer proposes this. Alternative worth raising with the customer: one E8 could do 2×CSI-2 (via the DSI-slave-mode trick) if only *one* of the two displays actually needs to be MIPI and the other can run off the CDC's parallel DPI/RGB output instead — cheaper than 2 full chips if their layout allows it.

### Clocking (relevant to multi-chip designs)
- LFXO: 32.768kHz external crystal
- HFXO: external crystal, 24–38.4MHz, **supports bypass mode for an external clock source**
- Internal: LFRC (~32.7kHz ±4%), HFRC (up to 76.8MHz ±2%)
- System PLL (main device clock) and a separate Audio PLL (I2S/PDM), both fast-locking fractional-mode multipliers
- Clock outputs routable to pins: HFXO_OUT, LFXO_OUT, LFRC_OUT, APLL_OUT

**Multi-chip sync note**: when a design uses two E8s that need to stay frame/timing-aligned (e.g. the 2-camera/2-display case above), there is **no documented multi-chip genlock or frame-sync pin** in the public datasheet — MHU/HWSEM (Alif's on-die IPC) do not span two separate packages. The practical approach: (1) frequency-lock both chips by feeding both HFXOs from one shared external oscillator, or bridging one chip's `HFXO_OUT` into the other's HFXO bypass input, so both System PLLs derive from the same reference; (2) layer a GPIO-based timing handshake on top (one chip toggles a GPIO on frame-start, the other treats it as an external-interrupt sync tick) if frame-level alignment is needed. This gets you "no visible drift" sync, not pixel-accurate hardware genlock — for genlock-grade requirements, get a reference design directly from Alif rather than assuming the silicon supports it.

### Peripherals (E8)
- 1x 10/100 Ethernet with DMA, 1x USB 2.0 HS/FS Host/Device with DMA, 1x SDIO v4.1 with DMA
- 1x CAN-FD (up to 10Mbps)
- MIPI I3C channel(s) (plus a low-power variant)
- 4x I2C (up to 3.4Mbps) + 2 low-power variants
- 8x UART (up to 5Mbps, 4 with RS-485 driver control) + 1 low-power variant
- 4x SPI (up to 50Mbps) + 1 low-power variant
- 2x 16-bit HexSPI (up to 400MB/s SDR / 800MB/s DDR, inline AES decryption, XiP + HyperBus support)
- GPIO: up to 136x 1.8V (shared with peripherals), up to 8x selectable 1.8V–3.3V, plus a handful of always-on/low-power GPIOs (4x 1.8V + 4x flex 1.8/3.3V)

These peripherals are the toolbox for inter-chip communication in a multi-E8 design (see multi-chip sync note above) — SPI/UART/I2C for a lightweight sync/handshake link, Ethernet or SDIO if bulk data needs to move between the two chips.
