# Purge System

PRINT_START integrates with a KAMP‑style purge macro using:
SETUP_LINE_PURGE LINE_LENGTH={purge_len} LINE_PURGE

---

## Adaptive Purge Length

Based on first‑layer height:
purge_len = 40mm * (layer_height / 0.2)

Clamped between:

- **20 mm minimum**
- **80 mm maximum**

---

## Why Adaptive Purge?

- Thin layers need less material  
- Thick layers need more  
- Prevents over‑extrusion on small nozzles  
- Ensures consistent first‑layer flow  