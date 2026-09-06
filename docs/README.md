# Alif Semiconductor Knowledge Base

Compiled Sept 2026 from https://alifsemi.com and https://github.com/alifsemi for use as a Senior FAE (Field Application Engineer) reference. This same content is also saved into the "Alif semi" Claude Project and packaged as the `alif-semi-fae` skill.

## Files
1. `00-overview.md` — company, product family map, links, known gaps
2. `01-ensemble-family.md` — E1C/E1/E3/E5/E7 specs & positioning
3. `02-ensemble-genai-family.md` — E4/E6/E8 (Ethos-U85 / generative AI)
4. `03-balletto-family.md` — B1 wireless family
5. `04-system-architecture.md` — RTSS-HP/HE, APSS, SESS, memory map, boot flow, MHU/HWSEM
6. `05-power-management-aiPM.md` — power modes, current/latency figures
7. `06-security-architecture.md` — Secure Enclave, lifecycle states, firewalls, SETOOLS
8. `07-software-tools-ides.md` — IDEs, toolchains, RTOS support, SDKs, Conductor, SETOOLS
9. `08-github-repos-reference.md` — full survey of github.com/alifsemi repos
10. `09-dev-kits-eval-boards.md` — evaluation kits by product family
11. `10-app-notes-index.md` — index of app notes/user guides (PDFs gated behind login)

## Maintenance notes
- Reference manuals/datasheets require an alifsemi.com login and could not be pulled automatically — the family/system-level facts here come from whitepapers, product pages, and GitHub READMEs, which are publicly accessible.
- GitHub org has 60 repos; ~30 were catalogued (the set consistently surfaced by the org's repo listing). Re-check https://github.com/orgs/alifsemi/repositories for anything newer or not covered here.
- Treat exact clock/memory/current numbers as "confirm against the datasheet for the specific part" rather than final — they were gathered from marketing/whitepaper-level sources, not the register-level reference manual.
