# CANCEL_PRINT Deep Dive

`CANCEL_PRINT` ensures a safe, predictable shutdown without double‑executing PRINT_END.

---

## Behavior Summary

1. Safe Z‑lift  
2. SMART_PARK  
3. Shut down:
   - nozzle
   - bed
   - chamber
   - fans  
4. Clear:
   - mesh
   - pause state  
5. Reset SD file  
6. Call `_BASE_CANCEL_PRINT`  

---

## Why Not Call PRINT_END?

PRINT_END is for successful prints.  
CANCEL_PRINT avoids:

- double Z‑lift  
- double park  
- double heater shutdown  

---

## Safety Notes

- Chamber fan is explicitly turned off  
- Extruder fan is turned off  
- Mesh is cleared to avoid stale compensation  