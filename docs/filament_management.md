# 📘 Filament Loading & Unloading
Material‑Aware • Tip‑Shaping • QIDI‑Safe
The macro suite includes enhanced LOAD_FILAMENT and UNLOAD_FILAMENT routines designed for reliability, clean filament handling, and compatibility with the QIDI X‑Plus 3’s constrained filament path.
These routines:
- Automatically select optimal temperatures based on material
- Shape the filament tip during unload (no cutting required)
- Perform controlled melt‑in during load
- Expose user‑tunable variables for advanced customization
- Maintain QIDI‑safe behavior (no auto‑resume, no double‑restore)


The suite includes two staged routines:

- FILAMENT_LOAD  
- FILAMENT_UNLOAD  

---

## FILAMENT_LOAD
### Behavior:
Controlled Melt‑In for Smooth Feeding
The load routine uses a two‑stage feed:
1. 	Fast Load
Quickly feeds filament through the Bowden/hotend path.
2. 	Slow Melt‑In
Gently feeds the filament into the melt zone to avoid deforming the shaped tip.
A final purge ensures clean extrusion.

### User‑Tunable Variables
`fast_load`    → fast feed distance
`slow_load`    → slow melt‑in distance
`purge_amount`  → purge amount after loading

This prevents:
- grinding  
- overshooting  
- inconsistent priming 

---

## FILAMENT_UNLOAD
### Behavior:

Tip‑Shaping for Clean Removal
The unload routine performs a three‑stage sequence to ensure a clean, snag‑free filament tip:
- Tip Push
A small forward extrusion rounds the filament tip.
- Slow Retract
A controlled retract shapes the tip and reduces stringing.
- Fast Retract
A high‑speed pull removes the filament cleanly from the melt zone and Bowden path.
This produces a clean, tapered tip that loads smoothly without cutting.

### User‑Tunable Variables
`tip_push`      → forward extrusion to round the tip
`slow_retract`  → slow retract distance for shaping
`fast_retract`  → fast retract distance for full removal

This prevents:
- heat‑break jams  
- filament snapping 
- need for cutting the fialment when doing load/unload or material changes, the warning on the QIDI LCD stating to cut and continue can be safely ignored.

---

## Internal Material Lookup
The  macro maps the  parameter to the correct load/unload temperatures. Unknown materials fall back to user defaults.
Contributors can extend this table to support additional materials or custom profiles you can find information about that in the [Material](material.md) documentation.