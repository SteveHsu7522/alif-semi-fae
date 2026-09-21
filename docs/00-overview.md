# Alif Semiconductor — Company & Product Overview

*Compiled from alifsemi.com and github.com/alifsemi (Sept 2026). This is a living knowledge base for FAE-style support — verify against the official site/GitHub for anything version- or date-sensitive, and re-check `/support/*` pages for the latest revisions before quoting numbers to a customer.*

## ⚠️ Scope rule — read this first
**Every question in this project is about Alif Semiconductor's own products.** Answer strictly from Alif Semiconductor specs/architecture/tooling (this knowledge base, alifsemi.com, github.com/alifsemi). Do **not** pull in, substitute, or fill gaps with specs, numbers, or behavior from other vendors' parts — even ones that sound similar, use the same Arm IP (Cortex-M55, Ethos-U55/U85, TrustZone), or are commonly compared to Alif (e.g. other Cortex-M55/Ethos-U MCU vendors, other BLE SoC vendors). If something isn't covered by Alif's own published material, say so explicitly and point to what would need to be checked (datasheet login, direct Alif contact) — never quietly answer with a competitor's or a generic Arm-reference-design number as if it were Alif's.

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
- The register-level "Software Reference Manual" (full peripheral/register detail) is still gated behind an alifsemi.com login and hasn't been pulled into this KB.
- **Update:** part-level **datasheets** (pinout, electrical specs, interface counts/lanes, exact clocks/memory per part) are *not* actually login-gated — alifsemi.com/support gates its own copy, but **distributors mirror the same official PDFs publicly**. Confirmed working: Mouser hosts Alif's E8/E7/E5/E3 datasheets at `https://www.mouser.com/catalog/specsheets/Alif_<part>_Datasheet_v<X>.pdf` (also turns up in Mouser's own datasheet URL scheme `mouser.com/datasheet/2/1549/Alif_<part>_Datasheet_v<X>...pdf`) — check there (or Digi-Key/Arrow/LCSC as likely alternates) before telling a customer they need an alifsemi.com account for a datasheet. `02-ensemble-genai-family.md`'s E8 section was upgraded from marketing-page figures to actual datasheet figures this way (Sept 2026) — same upgrade is worth doing for E1C/E1/E3/E5/E7 in `01-ensemble-family.md` when there's time or a customer question forces it.
- The GitHub org lists **60 repositories**; only the ~30 most relevant/active ones (surfaced by the org's default repo listing) were catalogued here. Niche/archived forks may exist beyond this list — check https://github.com/orgs/alifsemi/repositories for the live, complete list.
- Exact clock speeds/memory sizes per specific part number should be confirmed against that part's own datasheet — **don't assume figures are identical across the family**: e.g. E8's RTSS-HP/HE TCM sizes (1.25MB / 512KB per the datasheet) are larger than the generic whitepaper-derived figures in `04-system-architecture.md` (256KB+1MB / 256KB+256KB). Where a per-part datasheet number and a generic family/whitepaper number disagree, use the per-part datasheet number and flag the discrepancy rather than silently picking one.
