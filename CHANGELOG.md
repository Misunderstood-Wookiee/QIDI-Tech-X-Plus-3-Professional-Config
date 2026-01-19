# Changelog

All notable changes to this project will be documented here.

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