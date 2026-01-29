# Changelog
All notable changes to this project will be documented here.

## [Unreleased] - 2026-01-29

### Changed

- Bed mesh configuration optimized in `Functionality/Adaptive_Mesh.cfg`:
  - Leveling speed reduced from 300mm/s to 150mm/s for improved accuracy
  - Horizontal move Z height lowered from 5mm to 2.6mm to reduce probing time
  - Mesh area expanded: min from (20,20) to (20,8), max from (260,260) to (270,265) for better coverage
  - `LINE_PURGE` macro purge height adjusted from 0.4mm to 0.3mm for better bed adhesion and first layer quality
- Probe configuration refined in `Hardware/Probe.cfg`:
  - Sample retract distance optimized from 3.0mm to 0.7mm for faster probing cycles
  - Z-offset reverted to 0.0mm (from 1.549mm calibration)
- Stepper motor homing optimized in `Hardware/Steppers.cfg`:
  - X-axis homing speed reduced from 100mm/s to 40mm/s for improved sensorless homing reliability
  - Y-axis configuration corrected: position_endstop from 298 to -18, position_max from 298 to 280, homing_speed from 100 to 40mm/s, and homing_positive_dir from True to False
  - Z-axis homing refined: position_min from -6 to -2, homing_speed from 15 to 10mm/s, homing_retract_dist from 3.0 to 8.0mm, second_homing_speed from 2 to 5mm/s
- TMC2209 motor driver currents adjusted in `Hardware/TMC2209.cfg`:
  - Extruder run_current reduced from 1.1A to 0.714A to prevent overheating
  - Z-axis run_current reduced from 0.45A to 0.285A for quieter operation
  - Y-axis driver_SGTHRS lowered from 97 to 85 for more sensitive sensorless homing
- Heater configurations refined in `printer.cfg`:
  - Heated bed min_temp adjusted from -100°C to -50°C for safety
  - Chamber heater max_delta reduced from 1.0 to 0.5 for tighter temperature control
- Movement configuration optimized in `printer.cfg`:
  - Z-axis max velocity increased from 15mm/s to 20mm/s for faster Z movements
- Resonance tester configuration improved in `printer.cfg`:
  - Probe point Z height lowered from 20mm to 10mm for better measurement accuracy
  - Added accel_per_hz: 75 for improved resolution
  - Added sweeping_period: 0 for continuous sweep mode
  - Added max_smoothing: 0.5 to limit excessive smoothing
- Idle timeout reduced from 86400s (24 hours) to 5400s (90 minutes) for better power management in `printer.cfg`

### Added

- G-code arc support enabled in `printer.cfg` with 1.0mm resolution for smoother curved movements
- Response system activated in `printer.cfg` with default echo type for better macro feedback

- `MOISTURE_DIAGNOSTIC` macro for filament moisture testing
  - Material-aware temperature control using `_GET_MATERIAL_TEMPS`
  - Single-speed and multi-speed diagnostic modes
  - Automatic logging to `/tmp/moisture_check.log` with timestamps
  - Safe nozzle parking before and after tests
  - Configurable extrusion lengths, feedrates, and park positions
- Documentation for moisture diagnostic system in `docs/moisture_diagnostic.md`

### Improved

- Documentation sidebar (`docs/_sidebar.md`) reorganized into logical categories
  - **Getting Started**: Overview and Material Configuration
  - **Core Print Macros**: PRINT_START, PAUSE & RESUME, CANCEL_PRINT
  - **Utility Macros**: SMART_PARK, Purge System, Lighting System
  - **Filament Operations**: Filament Management, Moisture Diagnostic
  - **Help & Resources**: Troubleshooting, Support
  - **Contributing**: Contributing Guidelines, Maintainers

### Fixed

- Jinja2 template syntax error in `MOISTURE_DIAGNOSTIC` macro
  - Corrected RUN_SHELL_COMMAND formatting to use single curly braces instead of double
  - Fixed "expected token ':', got '}'" error during macro loading

## [1.2.2] - Bug fixes for Filament Mangement (thanks to qidi klipper firmware qwirks), Debbuging helpers and much needed QoL optimisations.

### Added

- Input Shaper section `[input_shaper]` for manual tuning.

### Improved
- TMC2209.cfg has some more in-line comments.

### Removed / Reverted

- Max Printer Speed reverted to factory 600 (might as well allow full range for travel moves) the changes to acceleration remain optimised.

## [1.2.0] - Bug fixes for Filament Mangement (thanks to qidi klipper firmware qwirks), Debbuging helpers and much needed QoL optimisations.

### Added

- Probe  `lift_speed: 10` for snappier multi‑sample probing.
- Full chamber‑lighting macro suite (LIGHT_ON, LIGHT_OFF, LIGHT_TOGGLE, LIGHT_BLINK).
- Integrated lighting cues into PRINT_START, PRINT_END, FILAMENT_LOAD, and FILAMENT_UNLOAD.
- Integrated buzzer cues into PRINT_START, PRINT_END, FILAMENT_LOAD, FILAMENT_UNLOAD, PAUSE, RESUME, and CANCEL_PRINT.
- Buzzer macro suite (BEEP, BEEP_LONG, BEEP_REPEAT) for clear audible feedback.
- Light + sound confirmation patterns for filament operations and print completion.
- `macros/qidi_debugger.cfg`; the start of this projects custom shared debugging suite of scripts _(currently it hold a macro to debug material temp selection)_.
- Optional wait for bed expansion just before G29 in PRINT_START 

### Improved

- README updated to reflect resonsibility of end user.
- Documentation in docs/materials.md to reflect the users ability to add/modify the WebUI Load/Unload temprature presets for materials.
- Cleaned and consolidated lighting macros into a dedicated, maintainable block.
- Replaced polar screw coordinates with accurate XY screw positions for more reliable manual bed leveling.
- Updated TMC2209 recommendations for X/Y, Z, and extruder to improve torque, accuracy, and high‑speed stability.
- Revised motion configuration guidance to replace unrealistic stock values with physically achievable, input‑shaper‑friendly limits.
- Reduced ringing and improved dimensional accuracy by recommending a lower square_corner_velocity.
- Overall print workflow now provides clearer visual and audible state feedback.
- Sensorless homing reliability
  - Added recommended diag_threshold tuning range (90–120) for both X and Y.
  - Improved stall detection consistency across varying belt tensions.
  - Reduced false triggers and over‑travel events.
  - Macro consistency



### Changed
- Y‑axis travel limits
  - Updated position_min from ‑24 to 0 to avoid the mechanical creak zone around Y‑20.
  - Ensures all macros operate within safe, positive coordinates.
  - Coordinate system cleanup
- All macros and motion routines now operate strictly within positive X/Y space.
- Eliminates accidental negative‑axis travel and improves safety.
- SMART_PARK macro
  - Added a safe, center‑bed park routine (X140 Y140) with configurable Z‑lift.
  - Integrated into PAUSE, CANCEL_PRINT, and PRINT_END workflows



### Removed / Reverted

- Motion‑activated lighting system (temporarily removed due to inconsistent behavior on QIDI‑locked firmware).
- All motion‑triggered light logic reverted to ensure predictable, stable operation.

### Fixed

- Ensured all lighting and buzzer macros operate consistently across all print states.
- Corrected screw positions in screws_tilt_adjust to probe near actual bed screw locations.
- While stock configuration allows for `stepper_y` to reach a shocking position_min: -24 it will crash into the front Y stops a more this has been fixed by setting  `position_min: -18` which is a more sensible value which does not over-extend the axis limits and should provide enough for wipers or other accessories.


## [1.1.8] - Bug fixes for Filament Mangement (thanks to qidi firmware qwirks again!) & Debugging Code along with QOL
### Added

- New FILAMENT_LOAD and FILAMENT_UNLOAD macros to replace M604/M603 due to QIDI firmware parameter restrictions.
- Material-aware temperature system with centralized lookup macro `_GET_MATERIAL_TEMPS`.
- Fluidd-friendly preset macros for PLA, PETG, ABS, ASA, and Nylon (load and unload).
- QIDI-compatible FILAMENT_DEBUG macro using action_respond_info.
- Optional M603/M604 wrapper macros for UI/LCD compatibility.

### Improved

- Documentation updated to reflect QIDI firmware limitations and new macro architecture.
- Clear descriptions added to all preset macros for better UI clarity.
- Material table and workflow documentation rewritten for clarity and contributor friendliness.

### Fixed

- Issue where M603/M604 always used default temps due to QIDI blocking parameter passing to M-code macros.
- Removed unsupported RESPOND command from debug macro.

## [1.1.5] - Tip Forming & Material-aware profiles for Filament Mangement
### Added

- Material‑aware filament handling system with automatic temperature selection
  (`MATERIAL=PLA|PETG|ABS|ASA|NYLON`).
- Tip‑shaping unload sequence (push → slow retract → fast retract) for clean,
  cut‑free filament removal.
- Controlled melt‑in load sequence with staged feed and purge.
- `_GET_MATERIAL_TEMPS` internal lookup macro for centralized material profile
  management.
- New helper macro `qidi_helpers.cfg` for handling shared macro functionality.

### Improved

- M604 and M603 rewritten for clarity, safety, and
  contributor‑friendly tuning.
- Documentation updated to reflect new material profiles, tip‑shaping behavior,
  and user‑tunable variables.

### Fixed

- Removed redundant temperature logic across load/unload macros.
- Ensured all filament routines remain QIDI‑safe with no auto‑resume or
  double‑restore behavior.

## [1.1.0] – Finalized Qidi‑Safe Pause/Resume System

### Fixed

- RESUME no longer stalls after returning to the paused layer.
- Corrected resume primitive: now calls `BASE_RESUME` (the actual Klipper/Qidi resume handler).
- Removed conflicting `RESTORE_GCODE_STATE NAME=PAUSE` logic.
- Eliminated double‑restore, double‑prime, and double Z‑hop behavior.
- Ensured PAUSE saves only park state (`PAUSEPARK`) and lets Klipper/Qidi manage the true resume point.
- Ensured RESUME:
  - restores from park
  - primes once
  - hands off to `BASE_RESUME` to resume the G‑code stream cleanly.

### Updated

- Documentation in `docs/pause_resume.md` to reflect the correct resume flow.
- README updated to clarify the correct resume primitive for Qidi firmware.
- Maintainer and contributor docs updated to highlight the correct resume handler.

### Added

- Diagnostic instructions for identifying resume primitives via `HELP`.

---

## [1.0.0] – Initial Release

### Added

- Full PRINT_START overhaul:
  - Adaptive purge length
  - Material‑aware chamber logic
  - Z‑offset compensation hook
  - Thermal‑expansion‑aware sequencing
  - SMART_PARK integration
- Unified SMART_PARK macro
- PRINT_END cleanup and safety improvements
- PAUSE macro rewrite:
  - Stores zhop and etemp
  - Safe Z‑hop
  - SMART_PARK integration
  - Prevents Qidi auto‑resume
- RESUME macro rewrite:
  - Restores from PAUSE and PAUSEPARK
  - Reheats to stored etemp
  - Single clean prime
  - Removed `_BASE_RESUME` to prevent double‑restore
- CANCEL_PRINT improvements:
  - Safe Z‑lift
  - SMART_PARK
  - Full heater/fan shutdown
  - No PRINT_END duplication
- FILAMENT_LOAD and FILAMENT_UNLOAD staged routines
- Integration with SETUP_LINE_PURGE for adaptive purge length

---

## [Unreleased]
### Planned

- Material presets (PLA/ABS/ASA/PETG)
- Optional wipe move after purge
- Long‑pause cooldown logic
- Resume purge option