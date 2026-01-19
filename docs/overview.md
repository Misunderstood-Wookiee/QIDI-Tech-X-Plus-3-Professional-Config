# Macro Suite Overview

This documentation explains the architecture, design goals, and behavior of the enhanced Klipper macro suite for the **Qidi X‑Plus 3**.

The macros are designed to be:

- **Qidi‑safe** — compatible with the X‑Plus 3’s custom firmware layer  
- **Predictable** — no double‑movement, no auto‑resume, no surprises  
- **Adaptive** — purge length, chamber behavior, and Z‑offset adjust dynamically  
- **Contributor‑friendly** — clean structure, clear variable flow, and consistent naming  

---

## Macro Architecture

The suite is built around a few core concepts:

### 1. **State‑Driven Behavior**
Macros store and restore state using:

- `SAVE_GCODE_STATE`
- `RESTORE_GCODE_STATE`
- macro variables (`zhop`, `etemp`)

This ensures consistent behavior across PAUSE, RESUME, and CANCEL_PRINT.

---

### 2. **Unified Parking Logic**
All macros that move the toolhead use:

- `SMART_PARK`

This ensures the machine always parks in a safe, predictable location.

---

### 3. **Material‑Aware Logic**
PRINT_START adapts based on:

- first‑layer height  
- bed temperature  
- chamber temperature  
- optional Z‑offset  

---

### 4. **Qidi‑Safe Overrides**
The built‑in Klipper commands:

- `PAUSE`
- `RESUME`
- `CANCEL_PRINT`

are renamed to:

- `_BASE_PAUSE`
- `_BASE_RESUME`
- `_BASE_CANCEL_PRINT`

This ensures the Qidi LCD buttons call the custom macros instead of the built‑ins.

---

## Macro Flow Diagram
PRINT_START ├─ Heat bed ├─ Heat chamber (if needed) ├─ Home → Mesh → SMART_PARK ├─ Heat nozzle └─ Adaptive purge → Start print
PAUSE ├─ Save state ├─ Z‑hop ├─ SMART_PARK ├─ Cool nozzle └─ Wait for user
RESUME ├─ Reheat nozzle ├─ Restore park state ├─ Prime └─ Restore print state → Continue
CANCEL_PRINT ├─ Z‑lift ├─ SMART_PARK ├─ Shutdown heaters/fans └─ Clear state

---

## Related Documents

- [print_start.md](print_start.md)
- [pause_resume.md](pause_resume.md)
- [cancel_print.md](cancel_print.md)
- [smart_park.md](smart_park.md)
- [filament_management.md](filament_management.md)
- [purge_system.md](purge_system.md)
- [troubleshooting.md](troubleshooting.md)
