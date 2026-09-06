# Ensemble Gen AI Family (E4, E6, E8) — "2nd generation"

Source: https://alifsemi.com/products/ensemblegenai/ and PR/whitepaper coverage of the Jan 2025 CES launch.

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
