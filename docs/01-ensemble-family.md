# Ensemble Family (E1C, E1, E3, E5, E7)

Source: https://alifsemi.com/products/ensemble/ (marketing-page level detail — confirm exact numbers per part with the datasheet).

## Series at a glance

| Series | Cores | NPU | Positioning | Notable interfaces |
|---|---|---|---|---|
| **E1C** | 1x Cortex-M55 | 0 or 1x Ethos-U55 (variant dependent) | Entry-level, lowest cost/power, wearables | Basic peripheral set, cost-optimized |
| **E1** | 1x Cortex-M55 | optional Ethos-U55 | Entry-level single real-time core, wearables | Up to 14 mics, dual-lane MIPI display |
| **E3** | 2x Cortex-M55 | optional 2x Ethos-U55 | Dual real-time core, security cameras / edge AI | Up to 22 mics, multiple image sensors |
| **E5** | 1x Cortex-A32 (800MHz, Linux) + 2x Cortex-M55 | (fusion) | Medical / high-end graphics + networking | 2x OctalSPI, SDHC |
| **E7** | 2x Cortex-A32 (800MHz) + 2x Cortex-M55 | 2x Ethos-U55 | Maximum performance — smart retail, industrial | 10/100 Ethernet |

"Fusion processor" = Alif's term for the parts that combine a Linux-capable Cortex-A32 application core with real-time Cortex-M55 core(s) on one die (E5, E7, and the Gen AI E4/E6/E8).

## Family-wide specs (as published)
- Memory: 128KB–13.5MB SRAM; 256KB–5.5MB on-chip MRAM (non-volatile, magneto-resistive), scaling by part
- AI performance: up to 250+ GOPS across the family (Ethos-U55, 256 MACs/clock in the HP config)
- Combined compute: 3,670+ DMIPS (top-end part)
- Every part includes: Secure Enclave subsystem (secure boot, key storage, crypto), aiPM autonomous power management, rich peripheral mix (I3C, CAN-FD, USB, Ethernet on select parts)
- Cortex-M55 = Armv8.1-M with Helium (MVE) vector extension — this is what accelerates DSP/ML workloads on the M-class cores even without the NPU.

## RTSS-HP vs RTSS-HE (applies across E1–E7 and Gen AI parts)
Each Cortex-M55 in the family sits in one of two subsystem flavors:
- **RTSS-HP** (High Performance): M55 up to 400MHz, 256KB I-TCM + 1MB D-TCM, Ethos-U55 with 256 MACs/cycle.
- **RTSS-HE** (High Efficiency): M55 up to 160MHz, 256KB+256KB TCM, Ethos-U55 with 128 MACs/cycle, lower leakage, can run **standalone** with minimal dependency on the rest of the chip (useful for always-on sensing while the rest of the SoC is powered down).

See `04-system-architecture.md` for full subsystem/memory-map detail and `05-power-management-aiPM.md` for the power-mode implications of HP vs HE.

## Positioning cheat-sheet for customer conversations
- Customer just needs a low-power sensor-fusion MCU with some local ML (wake-word, gesture, simple CV) → **E1/E1C**.
- Customer needs two independent real-time domains (e.g. one core does audio/vision preprocessing, another does control) with more mic/camera inputs → **E3**.
- Customer needs Linux (rich UI, networking stacks, complex file systems) *and* deterministic real-time cores on one chip, with high-end graphics or medical-grade I/O → **E5**.
- Customer needs the top of the line — dual Linux-capable cores, dual real-time cores, dual NPUs, Ethernet — for industrial/retail with heavier local AI → **E7**.
- Customer explicitly asks about running an LLM/transformer/generative model at the edge → point to Gen AI family (`02-ensemble-genai-family.md`), not base Ensemble.
