# Alif Semiconductor — Company & Product Overview

*Compiled from alifsemi.com and github.com/alifsemi (Sept 2026). This is a living knowledge base for FAE-style support — verify against the official site/GitHub for anything version- or date-sensitive, and re-check `/support/*` pages for the latest revisions before quoting numbers to a customer.*

## Who they are
Alif Semiconductor makes ARM-based, AI-enabled microcontrollers and fusion (MCU+MPU) processors aimed at battery-powered edge/endpoint devices: wearables, hearables, fitness, medical, smart retail, industrial and security-camera applications. Their pitch is "HW-accelerated AI that's 40x faster than conventional MCUs" combined with aggressive low-power design (**aiPM** — Autonomous Intelligent Power Management) and an on-die **Secure Enclave** in every device.

## Product families

| Family | Positioning | Core mix | NPU | Doc |
|---|---|---|---|---|
| **Ensemble** (E1C, E1, E3, E5, E7) | Edge AI | Cortex-M55 (RTSS-HP/HE), optional dual Cortex-A32 (E5/E7) | Ethos-U55 (single or dual) | `01-ensemble-family.md` |
| **Ensemble Gen AI** (E4, E6, E8) | Edge AI + Generative AI | Cortex-M55 x2 + Cortex-A32 x2 | Ethos-U55 x2 **+ Ethos-U85** (transformer-capable) | `02-ensemble-genai-family.md` |
| **Balletto** (B1) | Edge AI + Wireless (BLE 5.3 / 802.15.4) | Cortex-M55 + RISC-V network core + Cortex-M0+ security core | Ethos-U55 | `03-balletto-family.md` |

## Cross-cutting architecture topics
- System architecture (RTSS-HP/HE, APSS, SESS, memory map, boot flow, MHU/HWSEM): `04-system-architecture.md`
- Power management / aiPM / power modes & current figures: `05-power-management-aiPM.md`
- Security architecture (Secure Enclave, lifecycle states, firewalls, SETOOLS): `06-security-architecture.md`

## Tools & software ecosystem
- IDEs, SDKs, RTOS support, Conductor tool, SETOOLS, toolchains: `07-software-tools-ides.md`
- GitHub org survey (alifsemi, ~60 repos, 30 catalogued in detail): `08-github-repos-reference.md`
- Evaluation kits / dev boards: `09-dev-kits-eval-boards.md`
- Application notes & user guides index: `10-app-notes-index.md`

## Key external links
- Main site: https://alifsemi.com
- Products: https://alifsemi.com/products/ensemble/ , /ensemblegenai/ , /balletto/
- Support portal: https://alifsemi.com/support/ (datasheets, reference manuals, software & tools, app notes — **reference manuals require a free account login**)
- Conductor (web config tool): https://conductor.alifsemi.com
- GitHub org: https://github.com/alifsemi

## Known gaps in this knowledge base
- Reference manuals (full datasheets, register-level "Software Reference Manual") are gated behind an alifsemi.com login and could not be downloaded by an automated agent — a registered account is needed to pull the PDFs.
- The GitHub org lists **60 repositories**; only the ~30 most relevant/active ones (surfaced by the org's default repo listing) were catalogued here. Niche/archived forks may exist beyond this list — check https://github.com/orgs/alifsemi/repositories for the live, complete list.
- Exact clock speeds/memory sizes per specific part number (e.g. E7 vs E5 exact SRAM) should be confirmed against the datasheet for the exact part being discussed with a customer — the figures here are family-level, gathered from marketing/whitepaper pages, not the register-level reference manual.
