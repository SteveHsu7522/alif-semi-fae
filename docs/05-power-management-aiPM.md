# Power Management — aiPM™

Source: https://alifsemi.com/whitepaper/power-saving-alif-microcontrollers/ and the system-architecture whitepaper. Also see app note "Ensemble MCU and Fusion Processors Power Modes" (gated doc, listed in `10-app-notes-index.md`).

## aiPM concept
**aiPM (Autonomous Intelligent Power Management)**: hardware-driven power management that "powers on only what's needed, when needed" — independent per-subsystem CPU state control, dynamic power-domain gating, selective clock scaling, and configurable memory retention, largely without CPU/software babysitting the transitions. Managed via SESS services.

## Two-level power model

### Per-subsystem CPU states
- **RUN** — clock active, full power
- **SLEEP** — core clock stopped, context retained, fast resume
- **OFF** — core + subsystem powered down, context lost, leakage eliminated

### System-wide SoC power modes
| Mode | Description |
|---|---|
| **GO** | ≥1 subsystem in RUN/SLEEP; all resources available |
| **READY** | all subsystems in SLEEP |
| **IDLE** | all subsystems in OFF |
| **STANDBY** | HP region powered down; HE region still active (this is the "always-on sensing on RTSS-HE only" mode) |
| **STOP** | only the Always-On region active — deepest practical sleep short of full power removal |

## Current consumption reference points (3.3V)
| Mode / retention option | Current |
|---|---|
| STANDBY mode | 65 µA |
| STOP mode (4KB retention) | 1.6 µA |
| Retain 256KB RTSS-HE TCM | 2.3 µA |
| Retain 512KB RTSS-HE TCM | 4.7 µA |
| Retain Secure Enclave SRAM | 1.8 µA |
| Retain 4KB Backup SRAM | 50 nA |
| Balletto B1 run current | 22 µA/MHz |
| Balletto B1 stop mode | 700 nA |

## Wakeup latency
- STANDBY → application running: **~5 µs**
- STOP → application running: **~786 µs**
- Power-on-reset → running: **~50 ms**

## FAE talking points
- These are the numbers to reach for when a customer asks "how long does my battery last" or "how fast can I wake from sleep to catch a sensor event" — STANDBY's ~5µs wake makes it attractive for event-driven designs that still want to save power between events, vs STOP which is for long idle periods where the ~786µs wake penalty doesn't matter.
- Retention current scales with how much TCM you choose to retain — a customer optimizing battery life should be pushed to retain only what their wake-up ISR actually needs, not the whole TCM by default.
- Always confirm current figures against the specific part's datasheet — these are the published reference/typical figures, and exact numbers vary slightly across the E1/E3/E5/E7/Gen-AI/Balletto lineup and by silicon revision.
- Power-mode software configuration is done partly via **Conductor** (GUI) and partly via the aiPM API exposed to application code — check `ensemble_SDK` / `sdk-alif` for the current aiPM driver/service API when a customer needs code-level control rather than a one-time GUI config.
