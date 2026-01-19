# SMART_PARK

A unified parking routine used by:

- PRINT_START  
- PAUSE  
- RESUME  
- CANCEL_PRINT  

---

## Goals

- Avoid front airflow  
- Avoid clips  
- Avoid bed contamination  
- Keep nozzle away from print  
- Provide a consistent reference point  

---

## Default Location
X = max_x - 10 Y = 10 Z = 30

This keeps the toolhead:

- high enough to avoid collisions  
- far enough from the print  
- close enough for quick resume