# Software, Tools & IDE Ecosystem

Source: https://alifsemi.com/support/software-tools/view-all/ plus GitHub repo READMEs.

## IDEs / build environments
- **VS Code** (primary, free path) — via CMSIS-Pack tooling. Extensions Alif recommends: Arm Environment Manager, Arm CMSIS csolution, Cortex-Debug, Microsoft C/C++ Extension Pack. Manual installs still needed: VS Code itself, Alif SE tools (SETOOLS), J-Link software.
- **Keil MDK** — commercial alternative, "Getting Started with Bare Metal Design Using Keil MDK" app note exists for it.
- Required base tooling either way: Git, CMake, Ninja, **cmsis-toolbox**, a toolchain (see below), Alif SE tools.

## Compilers / toolchains
- **Arm GNU toolchain (GCC)** — default in most Alif templates (e.g. `alif_vscode-template`)
- **Arm Compiler for Embedded (Arm Clang)**
- **LLVM Clang**

## RTOS / software stacks supported
- **Zephyr RTOS** — first-class support: `sdk-alif` (Alif's Zephyr SDK, current published version referenced as v2.3.0), `zephyr_alif` (Alif's port of upstream Zephyr), `hal_alif` (HAL integration layer for Zephyr). Requires **Git + West**; `west.yml` manages module dependencies. Use tagged releases (e.g. `v2.3.0`), not `main`, for anything stable — `main` is active development.
- **Eclipse ThreadX** — supported per the Ensemble SDK software list.
- **FreeRTOS** — supported; there's a dedicated "Alif FreeRTOS CMSIS Support Pack" with drivers + design examples.
- **Bare-metal** (no RTOS) — the CMSIS-DFP + VS Code template path targets this directly.
- **Linux (APSS / Cortex-A32 side)** — Yocto-based: `meta-alif` (BSP layer), `meta-alif-ensemble` (machine configs, TF-A, kernel recipes), `linux_alif` (Alif's kernel fork — as of the last check its `main` branch was an explicit placeholder, "work in progress," so check for the actual working branch/tag before relying on it), `trusted-firmware-a_alif` (TF-A secure boot chain port for the A32 side). App note "Linux APSS User Guide" covers the officially supported release flow (referenced v2.0.0 for E8).

## Core SDKs
- **ensemble_SDK** (a.k.a. "Ensemble SDK" on GitHub) — the general-purpose SDK: guides, config tools, example apps, templates. Supports E7/E5/E3/E1/E1C/B1, primarily targeting the M55 HP/HE cores in the RTSS. Toolchain-agnostic (VS Code + Keil, GCC/Arm-Clang/LLVM-Clang), RTOS-agnostic (Zephyr/ThreadX/FreeRTOS/bare-metal).
- **Balletto SDK** — analogous SDK for B1 (separate GitHub entry referenced from the software-tools page; verify current repo name/URL on github.com/alifsemi before pointing a customer at it, as it wasn't in the default 30-repo listing captured for this KB).
- **alif_ensemble-cmsis-dfp** — the CMSIS Device Family Pack: register/peripheral drivers for Ensemble parts. **v2.0.x is NOT backward compatible with v1.3.4 or older** — if a customer's project fails to build after a DFP update, check for this exact break first, and point them at Application Note AAPN0038 ("CMSIS DFP 2.0 Migration Guide").
- **alif_cmsis_packs** — index repo pointing at all of Alif's published CMSIS packs.

## Configuration tools
- **Conductor** (https://conductor.alifsemi.com) — web-based GUI for security config, peripheral/pin-mux setup, clock tree, and power evaluation.
- **Offline Conductor** (currently v1.0.17) — same tool, installable on Linux/Windows for offline use (e.g. no-internet lab environments, or IP-sensitive customers).

## Security tooling
- **Alif Security Toolkit (SETOOLS)**, current v1.110.00 — see `06-security-architecture.md`.
- **SE Host Services API**, current v1.109.0/v1.110.00 — host-side API + source archive.

## Graphics / media stack
- **D/AVE2D** — 2D GPU accelerator on Ensemble parts. Driver: `alif_dave2d-driver` (3-layer architecture: HW-specific L1, device-independent L2, memory-management L0).
- **alif_image-processing-lib** — CMSIS-pack image-processing library built on D/AVE2D (crop/flip/resize/rotate, color matrix correction, white balance, LUTs; 18 supported pixel formats spanning RGB/YUV/Bayer). Depends on `Dave2DDriver@1.0.1`.
- **LVGL** integration — `alif_lvgl-dave2d` (LVGL-to-D/AVE2D integration layer) and `alif_m55-lvgl` (ready-made LVGL example for an Ensemble DevKit). Alif also mirrors upstream `lvgl` itself in the org.

## Connectivity / USB
- **TinyUSB** support: `tinyusb` (Alif's port of the upstream cross-platform USB stack) and `alif_tinyusb-examples` (CDC+MSC, HID composite, HID boot, HID multi-interface, MSC dual-LUN — each in bare-metal / FreeRTOS / Zephyr variants where applicable; targets the E7 DK's rtss_hp and rtss_he cores).

## ML / AI tooling
- **alif_ml-embedded-evaluation-kit** — Alif's fork of Arm's ML Embedded Evaluation Kit, extended with a curated set of ML models runnable on Alif boards. Default target is the AppKit; other boards selected via `TARGET_BOARD`. Supports MT9M114 (default), ARX3A0, OV5675 camera modules for vision demos.
- **alif_mlek-cmsis-examples** — Alif port of Arm's MLEK CMSIS-pack examples (marked **archived** — treat as reference only, not actively maintained).

## Production / manufacturing
- **alif-sdk-containers** — Docker containers used to build "ZAS" releases (Alif's internal release-build tooling) — mostly relevant if a customer wants to reproduce Alif's own build environment rather than using released binaries.
- Provisioning workflow guidance: app note "Provisioning Production Workflow" (binary image provisioning + field/OTA updates) and "SERAM Update Procedure."

## Design resources
- **Altium Design Library** (schematic/PCB elements for Ensemble & Balletto, BGA/CSP variants) — dated 04-16-2026 at last check.
- PCB layout app notes exist per family: E1C, E1/E3/E5/E7, E4/E6/E8, and Balletto B1 each have their own guideline doc, plus a dedicated RF-shielding note for B1 (radio-adjacent layout is more sensitive).

## FAE quick-routing table
| Customer situation | Point them at |
|---|---|
| First bring-up, wants easiest path | VS Code + `alif_vscode-template` + Conductor |
| Wants an RTOS with strong device-tree/driver ecosystem | Zephyr (`sdk-alif`) |
| Already has a FreeRTOS codebase to port | FreeRTOS CMSIS Support Pack |
| Needs Linux + real-time cores together | `ensemble_SDK` (M55 side) + `meta-alif`/`meta-alif-ensemble` (A32/Linux side) |
| Build fails after updating CMSIS pack | Check DFP 1.3.4→2.0.x breaking change, AAPN0038 |
| Wants to run a vision/audio ML model | `alif_ml-embedded-evaluation-kit` |
| Needs USB device functionality | `alif_tinyusb-examples` |
| Needs a GUI on-device | LVGL + `alif_lvgl-dave2d` / `alif_m55-lvgl` |
| Production provisioning / secure boot setup | SETOOLS + "Provisioning Production Workflow" app note |
