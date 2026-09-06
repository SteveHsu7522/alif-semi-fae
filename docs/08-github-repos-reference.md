# GitHub Org Reference — github.com/alifsemi

The org lists **60 repositories total**. GitHub's default repo-list view (and every pagination attempt during this survey) surfaced the same **30 repos** below — treat this as "the actively surfaced/most relevant set," not a guaranteed-complete list. For anything not here, browse https://github.com/orgs/alifsemi/repositories directly.

Default branch note: several repos use `main`, at least one (`meta-alif`, `meta-alif-ensemble`) returned 404 on both `main` and `master` for a direct README fetch — check the repo's actual default branch in the GitHub UI before scripting against raw.githubusercontent.com URLs.

## Firmware / MCU SDK & HAL
| Repo | Description |
|---|---|
| `ensemble_SDK` | General-purpose Ensemble SDK — guides, config tools, examples, templates. Supports E7/E5/E3/E1/E1C/B1, VS Code + Keil, GCC/Arm-Clang/LLVM-Clang, Zephyr/ThreadX/FreeRTOS/bare-metal. |
| `alif_ensemble-cmsis-dfp` | CMSIS Device Family Pack for Ensemble. v2.0.x breaks compat with v1.3.4-; see AAPN0038 migration guide. |
| `alif_cmsis_packs` | Index of Alif's published CMSIS packs. |
| `alif_vscode-template` | Minimal CMSIS-Pack VS Code starter: LED blink, UART printf-retarget, SEGGER RTT printf-retarget examples. Uses Arm GNU toolchain by default (switchable to Arm LLVM). |

## Zephyr ecosystem
| Repo | Description |
|---|---|
| `sdk-alif` | Alif's Zephyr SDK — comprehensive dev environment for Alif devices on Zephyr. Needs Git+West; use release tags (e.g. v2.3.0), not `main`. |
| `zephyr_alif` | Alif's port of upstream Zephyr RTOS. |
| `hal_alif` | HAL integration layer bridging Alif hardware to Zephyr's HAL interfaces. |
| `aiPM_demo` | (Repo description on GitHub currently just says "Zephyr SDK" — likely an aiPM-focused demo built on sdk-alif; confirm actual contents before quoting scope to a customer.) |

## Linux / Yocto (APSS, Cortex-A32 side)
| Repo | Description |
|---|---|
| `linux_alif` | Alif's Linux kernel fork for APSS. **Caution:** at last check its `main` branch was an explicit placeholder ("Work in progress. main is a temporary empty branch.") — find the real working branch/tag before use. |
| `meta-alif` | Yocto BSP layer for the Ensemble family. |
| `meta-alif-ensemble` | Machine configs, TF-A integration, and Linux kernel recipes for Ensemble devices. |
| `trusted-firmware-a_alif` | Arm Trusted Firmware, Alif port — secure boot chain for the A32/APSS side. |
| `alif_a32_linux_DD_testcases` | Userspace Linux device-driver test cases for Ensemble. |
| `alif-sdk-containers` | Docker containers for building Alif's internal "ZAS" releases. |

## USB
| Repo | Description |
|---|---|
| `tinyusb` | Alif's port of the TinyUSB cross-platform USB stack. |
| `alif_tinyusb-examples` | CDC+MSC, HID composite, HID boot, HID multi-interface, MSC dual-LUN examples; bare-metal/FreeRTOS/Zephyr variants; targets E7 DK rtss_hp/rtss_he. |

## Graphics / GUI / Image processing
| Repo | Description |
|---|---|
| `alif_dave2d-driver` | D/AVE2D 2D-accelerator driver (3-layer: HW-specific / device-independent / memory-management). |
| `alif_image-processing-lib` | Image-processing library on D/AVE2D + Helium: crop/flip/resize/rotate, color correction, 18 pixel formats. Depends on Dave2DDriver@1.0.1. |
| `alif_lvgl-dave2d` | LVGL integration layer over D/AVE2D. |
| `alif_m55-lvgl` | Ready-made LVGL example for an Ensemble M55 DevKit. |
| `lvgl` | Mirror of the upstream LVGL graphics library. |

## AI / ML
| Repo | Description |
|---|---|
| `alif_ml-embedded-evaluation-kit` | Fork of Arm's ML Embedded Evaluation Kit with Alif-curated models; default target AppKit, `TARGET_BOARD` selects others; camera options MT9M114/ARX3A0/OV5675. |
| `alif_mlek-cmsis-examples` | Alif port of Arm's MLEK CMSIS-pack examples. **Archived** — reference only. |
| `alif_M55-viewfinder` | Viewfinder demo application (camera/vision demo, likely pairs with the ML eval kit or image-processing lib). |

## Power / low-power demos
| Repo | Description |
|---|---|
| `alif_LowPowerDemo_AudioCapture` | Single-core template: minimum device config for continuous audio capture (low-power always-on audio use case). |
| `alif_LowPowerDemo_StopMode` | Multicore template: power-mode transitions + core-to-core comms, focused on STOP mode. |
| `alif_LowPowerDemo_StandbyMode` | Multicore template: power-mode transitions + wakeup events, focused on STANDBY mode. |

## Services / misc
| Repo | Description |
|---|---|
| `SERVICES_aiPM_CLI` | CLI tool related to aiPM services (no GitHub description captured — inspect directly before referencing specifics). |
| `test_results` | Central place collecting CI test-result badges linked from other repos — not customer-facing. |
| `.github` | Org-level GitHub config/description repo — not customer-facing. |

## FAE usage notes
- When a customer asks "where's the example for X," this table is the fast index — but always open the actual repo/README before promising specific file names or APIs, since repos evolve.
- Several repos are thin wrappers/forks of upstream open-source projects (TinyUSB, LVGL, Arm's ML eval kit, Arm Trusted Firmware) — when troubleshooting, check whether an issue is Alif-specific (their port/HAL layer) or inherited from upstream (in which case upstream's issue tracker/docs may be more useful than Alif's).
- `alif_mlek-cmsis-examples` is explicitly archived — don't lead a customer to start new work there; it's fine as historical reference.
