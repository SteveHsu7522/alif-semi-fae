# Alif Semiconductor FAE Knowledge Base

A living reference for working with Alif Semiconductor's Ensemble, Ensemble Gen AI, and Balletto product lines — built from https://alifsemi.com and https://github.com/alifsemi, structured for use as a Claude Skill and as a general engineering reference.

## Contents
- **`docs/`** — the knowledge base itself (product families, system architecture, power management, security, software/tools, GitHub repo survey, dev kits, app-notes index). Start at `docs/README.md`.
- **`skills/alif-semi-fae/SKILL.md`** — a Claude Skill that packages this knowledge base for FAE-style Q&A (product selection, architecture, power, security, toolchain routing).

## Status / maintenance
- Compiled September 2026. Alif ships new parts, SDK releases, and tool versions regularly — treat exact specs/versions in `docs/` as "confirm against the current datasheet/repo," not final.
- Reference manuals and datasheets require a (free) alifsemi.com account login and are **not** mirrored here — only publicly accessible whitepaper/product-page/README-level information is included.
- The alifsemi GitHub org has ~60 repos; `docs/08-github-repos-reference.md` catalogues the ~30 most relevant ones surfaced by the org's default listing. Check https://github.com/orgs/alifsemi/repositories for anything newer.

## Updating this repo
When new Alif material is gathered (new product launch, new repo, updated app note, etc.), add or update the relevant file under `docs/`, keep `skills/alif-semi-fae/SKILL.md` in sync with anything that changes the condensed cheat-sheets, and commit.
