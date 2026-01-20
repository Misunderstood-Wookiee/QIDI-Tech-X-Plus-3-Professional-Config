# 📘 Filament Management System
Material‑Aware Loading & Unloading for QIDI X‑Plus 3
This system provides a safe, predictable, material‑aware filament workflow that works reliably on QIDI’s locked Klipper firmware. It includes:

- Material‑aware temperature selection
- Clean tip‑shaping unload
- Controlled melt‑in load
- Fluidd‑friendly preset buttons
- QIDI‑compatible debug macro
- Wrapper macros for M603/M604 (optional)
  - Because QIDI firmware blocks parameter passing to M‑code macros, the real logic lives in:

```
FILAMENT_LOAD
FILAMENT_UNLOAD
```
M603/M604 are optional wrappers that forward parameters correctly.

## Overview of Workflow
1. 	User selects a material (PLA, PETG, ABS, ASA, Nylon)
2. 	 maps the material to load/unload temps
3. 	 performs tip‑shaping and retract
4. 	 performs melt‑in and purge
5. 	Optional preset buttons call the correct material automatically
6. 	 prints the temps for verificatio

## Internal Material Lookup
The _GET_MATERIAL_TEMPS macro:
- Converts MATERIAL to uppercase
- Selects the correct temps
- Stores them in variable_load and variable_unload
- Falls back to defaults if unknown
This keeps all material logic centralized.
Contributors can extend this table to support additional materials or custom profiles you can find information about that in the [Material](material.md) documentation.

## FILAMENT_UNLOAD (Tip‑Shaping Unload)
This macro:
- Heats to the material‑specific unload temp
- Performs a tip‑push
- Performs a slow retract to shape the tip
- Performs a fast retract to fully remove filament
- Leaves the nozzle cold
### User‑Tunable Variables
`tip_push`      → forward extrusion to round the tip
`slow_retract`  → slow retract distance for shaping
`fast_retract`  → fast retract distance for full removal

# FILAMENT_LOAD (Controlled Melt‑In Load)
This macro:

- Heats to the material‑specific load temp
- Performs a fast feed
- Performs a slow melt‑in
- Purges a small amount
- Leaves the nozzle hot

User‑tunable variables:

```
fast_load
slow_load
purge_amount
```
## Material Preset Buttons
These appear as buttons in Fluidd/Mainsail and call the correct material profile
### Load Presets
```
LOAD_PLA      → FILAMENT_LOAD MATERIAL=PLA
LOAD_PETG     → FILAMENT_LOAD MATERIAL=PETG
LOAD_ABS      → FILAMENT_LOAD MATERIAL=ABS
LOAD_ASA      → FILAMENT_LOAD MATERIAL=ASA
LOAD_NYLON    → FILAMENT_LOAD MATERIAL=NYLON
```
### Unload Presets
```
UNLOAD_PLA    → FILAMENT_UNLOAD MATERIAL=PLA
UNLOAD_PETG   → FILAMENT_UNLOAD MATERIAL=PETG
UNLOAD_ABS    → FILAMENT_UNLOAD MATERIAL=ABS
UNLOAD_ASA    → FILAMENT_UNLOAD MATERIAL=ASA
UNLOAD_NYLON  → FILAMENT_UNLOAD MATERIAL=NYLON
```
Each preset includes a description so it appears clearly labeled in the UI.

# Debug Macro (QIDI‑Compatible)
QIDI firmware does not support `RESPOND`, so the debug macro uses
```
action_respond_info
```
This prints the selected material and temps to the console.
Example:
```
FILAMENT_DEBUG MATERIAL=PETG
```
Output:
```
Material: PETG
Load Temp: 230
Unload Temp: 210
```

## Optional: M603/M604 Wrappers
If you want to keep the familiar M‑code buttons in Fluidd:
```
M603 → FILAMENT_UNLOAD
M604 → FILAMENT_LOAD
```
