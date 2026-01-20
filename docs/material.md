## 🎨 Materials & Temperature Profiles

Filament Handling for the QIDI X‑Plus 3 Macro Suite
This page documents the material‑aware temperature system used by the filament load and filament unload  macros. These routines automatically select optimal temperatures for each material type and apply safe, predictable sequences for loading and unloading filament on the QIDI X‑Plus 3.

The goal is simple:
Fast, reliable filament changes with clean tips, no cutting, and no jams.

## Overview

The macro suite supports material‑specific temperature presets. When a user specifies a material, the macros automatically apply the correct load and unload temperatures.
Example usage:
`LOAD_FILAMENT MATERIAL=PETG`
`UNLOAD_FILAMENT MATERIAL=ASA`

## Supported Materials

- PLA     → Load 200°C / Unload 185°C
- PETG    → Load 230°C / Unload 210°C
- ABS/ASA → Load 245°C / Unload 230°C
- Nylon   → Load 260°C / Unload 240°C
- Other   → Falls back to user defaults

### These values were chosen to balance:

- clean tip formation
- minimal stringing
- safe melt‑in behavior
- compatibility with the QIDI X‑Plus 3’s constrained filament path
Contributors may extend or override these profiles as needed.

---

# How Material Selection Works

Both  and  call an internal helper macro:
`_GET_MATERIAL_TEMPS MATERIAL=<type>`

This macro:

1. Converts the material name to uppercase
2. Looks up the correct load/unload temperatures
3. Stores them in internal variables
4. Falls back to user defaults if the material is unknown
This keeps the logic centralized and easy to maintain.

## Extending Material Profiles

Contributors can add new materials by editing the `_GET_MATERIAL_TEMPS` macro and adding a new block:

```
{% elif mat == "materialname" %}
    {% set load = loadtemp %}
    {% set unload = unloadtemp %}
```
located inside the qidi_helpers script.
