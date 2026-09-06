# Evaluation Kits / Dev Boards

Source: https://alifsemi.com/support/kits/

| Kit | Chip | Category | Notes |
|---|---|---|---|
| **E8 DevKit** | Ensemble E8 | Edge AI + Gen AI | Flagship board for the transformer-capable (Ethos-U85) Gen AI family |
| **E7 DevKit** | Ensemble E7 | Edge AI | General-purpose top-of-line Ensemble evaluation board |
| **E7 AI/ML AppKit** | Ensemble E7 | Edge AI (AI/ML-specialized) | The default target board for `alif_ml-embedded-evaluation-kit`; likely includes camera/mic peripherals for vision+audio ML demos |
| **E1C DevKit** | Ensemble E1C | Edge AI | Entry-level evaluation board |
| **B1 DevKit** | Balletto B1 | Edge AI + Wireless | BLE/802.15.4 wireless evaluation board |

Detailed per-kit specs (schematic, exact peripherals, pinout) were not exposed on the summary page — pull the kit-specific page on alifsemi.com or its Quick Start Guide before a customer bring-up call.

## FAE routing
- ML/vision demo request → **E7 AI/ML AppKit** (matches the ML eval kit's default target).
- Generative AI / transformer demo request → **E8 DevKit**.
- Wireless/audio (earbuds, hearables) demo request → **B1 DevKit**.
- Lowest-cost entry evaluation → **E1C DevKit**.
- General Ensemble bring-up / broadest peripheral coverage → **E7 DevKit**.

## Debug hardware
- **SEGGER J-Link** is the standard debug probe across the ecosystem (VS Code template setup calls it out explicitly as a manual install alongside SE tools).
- **Segger Ozone** is documented as an alternative/companion debugger — see app note "Segger Ozone and J-Link Debug" for standalone debugging setup outside VS Code/Keil.
