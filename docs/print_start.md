# PRINT_START Deep Dive

`PRINT_START` is the most complex macro in the suite.  
It handles:

- homing  
- chamber heating  
- bed heating  
- Z re‑homing  
- adaptive mesh  
- SMART_PARK  
- nozzle heating  
- adaptive purge  
- optional Z‑offset  

---

## Parameter Reference

| Parameter | Default | Description |
|----------|---------|-------------|
| `BED_TEMP` | 70 | Bed temperature |
| `EXTRUDER_TEMP` | 220 | Nozzle temperature |
| `CHAMBER_TEMP` | 0 | Chamber target (optional) |
| `LAYER` | 0.2 | First‑layer height |
| `Z_ADJUST` | 0.0 | Optional Z‑offset |

---

## Adaptive Purge Logic

The purge length scales with first‑layer height:
purge_len = base_length * (layer_height / 0.2)

Clamped between **20 mm** and **80 mm**.

This ensures:

- thin first layers don’t over‑purge  
- thick first layers get enough material  

---

## Chamber Logic

If `BED_TEMP >= 90` (ABS/ASA):

- chamber defaults to **45°C** unless overridden

If `BED_TEMP < 90` (PLA/PETG):

- chamber stays **off** unless explicitly set

---

## Z‑Offset Hook

If the slicer passes:
SET_GCODE_OFFSET Z={value}
PRINT_START applies it **after** probing and mesh generation.

---

## Flow Summary

1. Home  
2. Heat bed  
3. Heat chamber  
4. Re‑home Z  
5. Mesh  
6. SMART_PARK  
7. Heat nozzle  
8. Adaptive purge  
9. Restore state → Begin print