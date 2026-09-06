# Balletto Family (B1) — Edge AI + Wireless

Source: https://alifsemi.com/products/balletto/

## What it is
Alif's wireless line: same DNA as Ensemble (Cortex-M55 + Helium + Ethos-U55 + Secure Enclave + aiPM) but with an integrated **multi-protocol radio** so customers don't need a separate connectivity chip.

## Core mix
- Cortex-M55 application CPU @ 160MHz (Helium/MVE vector extension)
- Ethos-U55 NPU — 128 MACs/cycle → 46 GOPS of ML acceleration
- Dedicated **RISC-V** core for the network/radio stack
- Cortex-M0+ core dedicated to security

## Wireless
- **Bluetooth Low Energy 5.3**
- **IEEE 802.15.4** (Thread/Zigbee/Matter-capable PHY/MAC layer)
- Positioned as multi-protocol without needing a companion radio SoC

## Memory & power
- 4MB integrated memory, 1:1 NVM/RAM ratio
- OctalSPI for external memory expansion
- aiPM figures: **22µA/MHz run current**, **700nA stop-mode current**
- Sub-16mm² package — aimed at extremely space-constrained designs

## Peripherals
- Audio DSP with **LC3 codec** hardware support (this is the codec used by Bluetooth LE Audio / TWS earbuds)
- Display: RGB / MIPI-DSI up to VGA resolution
- Up to 14 microphones, multiple digital/analog sensor interfaces

## Target applications
- TWS (true wireless stereo) LE Audio earbuds
- Hearing aids
- Fitness trackers with sensor fusion
- Mobile point-of-sale terminals

## FAE talking points
- Balletto is the answer when a customer needs **BLE Audio (LC3) + on-device ML (wake word, keyword spotting, sensor fusion) + tiny form factor** in one chip — e.g. earbuds that do local voice activity detection or noise classification without a host SoC round-trip.
- The RISC-V network core and M0+ security core running independently of the main M55 app core means the app core can sleep while connectivity/security housekeeping continues — ask about the customer's duty-cycle requirements to size which aiPM state fits.
- Dev platform: **B1 DevKit** (see `09-dev-kits-eval-boards.md`).
