# Application Notes & User Guides — Index

Source: https://alifsemi.com/support/application-notes-user-guides/view-all/ (titles/descriptions only — full PDFs require an alifsemi.com login; datasheets and reference manuals are similarly gated).

## User Guides
1. **Alif Security Toolkit** — full SETOOLS command reference (Ensemble & Balletto)
2. **SE Host Services API** — API reference for the SE Host Services software package
3. **Linux APSS User Guide** — working with the Linux APSS release (referenced v2.0.0, DK-E8)
4. **Linux Pre-Built Images Archive** — pre-built Linux images for DK-E8
5. **APSS Linux Utilities** — Flash-Tool app for programming Linux images to OSPI flash
6. **Ensemble VS Code Project Template** — VS Code template usage guide
7. **Getting Started with Bare Metal Design Using VS Code** — bare-metal M55 design walkthrough
8. **Getting Started with Bare Metal Design Using Keil MDK** — same, for Keil

## Application Notes
1. **Balletto B1 PCB Layout Guidelines** — power distribution & signal integrity
2. **RF Shielding PCB Design Guidelines for Balletto B1** — RF shield integration
3. **Ensemble E1C PCB Layout Guidelines**
4. **Power Measurement on the Ensemble E8 DevKit** — board mods for power-domain measurement
5. **Provisioning Production Workflow** — binary image provisioning, field/OTA updates
6. **SERAM Update Procedure** — safely updating SERAM images
7. **Ensemble MCU and Fusion Processors Power Modes** — power-saving features detail (companion to `05-power-management-aiPM.md`)
8. **Conductor Tool** — device configuration tool functions/setup
9. **Importing Example Projects or Source Code Templates into VS Code**
10. **Ensemble CMSIS Examples and Templates** — catalog of available example projects
11. **Ensemble E1, E3, E5, E7 PCB Design Guidelines**
12. **Ensemble E4, E6, E8 PCB Layout Guidelines**
13. **Ensemble Altium Design Library** — schematic/PCB elements, BGA/CSP variants
14. **Segger Ozone and J-Link Debug** — debugger/probe configuration

## Referenced-but-not-listed
- **AAPN0038 — CMSIS DFP 2.0 Migration Guide** (mentioned on the DFP repo README, not seen in the app-notes list scrape — search the support portal directly by this code if needed)

## FAE usage
- These require a (free) alifsemi.com account to download the PDF. If a customer doesn't have one, direct them to register at alifsemi.com/support — don't attempt to source these documents through any unofficial channel.
- When answering a PCB layout / hardware design question, match the customer's exact family (E1C guidelines differ from E1/E3/E5/E7, which differ again from E4/E6/E8) — don't assume one PCB app note covers all Ensemble parts.
