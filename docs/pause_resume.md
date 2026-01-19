# PAUSE & RESUME Deep Dive

The PAUSE/RESUME pair is designed to be:

- Qidi‑safe  
- state‑driven  
- predictable  
- free of double‑movement  

---

## Why Qidi Needs Special Handling

The Qidi LCD calls:

- `PAUSE`
- `RESUME`

directly.

To intercept these, the macros rename the built‑ins:
rename_existing: _BASE_PAUSE rename_existing: _BASE_RESUME

This ensures the LCD buttons trigger the custom macros.

---

## PAUSE Behavior

### Actions:

1. Store:
   - `zhop`
   - `etemp`
2. Save print state
3. Safe Z‑hop
4. SMART_PARK
5. Cool nozzle
6. Disable extruder stepper
7. Set long idle timeout
8. Call `_BASE_PAUSE` **at the end**

### Why `_BASE_PAUSE` is last

If called early, Qidi firmware auto‑resumes.  
Placing it last prevents this.

---

## RESUME Behavior (Final)

The Qidi firmware exposes the original Klipper resume handler as:
`BASE_RESUME`

This command:
- restores the internal paused state
- resumes the G‑code motion queue
- does **not** perform any movement, priming, or Z‑hop

The custom RESUME macro therefore:
1. Reheats to the stored nozzle temperature  
2. Restores from `PAUSEPARK` to leave the park position  
3. Primes once  
4. Calls `BASE_RESUME` to resume the print  

This is the only combination that:
- avoids double‑movement  
- avoids double‑prime  
- avoids double Z‑hop  
- resumes the print cleanly  
- works from the Qidi LCD  
---

## State Diagram
PAUSE ├─ Save PAUSE ├─ Z-hop ├─ SMART_PARK └─ Save PAUSEPARK
RESUME ├─ Restore PAUSEPARK ├─ Prime └─ Restore PAUSE
