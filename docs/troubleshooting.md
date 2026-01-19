# Troubleshooting

Common issues and solutions.

## Printer is not setting extruder or bed or chamber temps when prints start
**Cause:** `Slicer configuration` is not configured correctly.
**Fix:** Ensure your `slicer program` is configured for passing klipper tempratures `EXTRUDER_TEMP`,`BED_TEMP`,`CHAMBER_TEMP`

### Common Slicer Examples
**Orca Slicer**
`PRINT_START EXTRUDER_TEMP=[nozzle_temperature_initial_layer] BED_TEMP=[hot_plate_temp_initial_layer] CHAMBER_TEMP={overall_chamber_temperature} LAYER=[initial_layer_print_height]`

---

## Printer returns to the paused layer but does not resume printing

**Cause:**  
The resume primitive was not the correct one for Qidi firmware.
**Fix:**  
Use `BASE_RESUME` at the end of the RESUME macro.

**Why:**  
`BASE_RESUME` is the actual Klipper/Qidi resume handler that resumes the G‑code stream without performing its own movement.

---

## Weird double‑prime or double Z‑hop on resume

**Cause:** `_BASE_RESUME` was still being called.  
**Fix:** Remove `_BASE_RESUME` from RESUME and call `RESUME_BASE` LAST


---

## Unknown variable 'zhop'

**Cause:** RESUME was missing `variable_zhop`.  
**Fix:** Add:
variable_zhop: 0

---

## Purge line too short or too long

**Cause:** Incorrect `LAYER` parameter from slicer.  
**Fix:** Ensure slicer passes correct first‑layer height.

---

## Chamber not heating

**Cause:** `BED_TEMP < 90` and no explicit `CHAMBER_TEMP` set.  
**Fix:** Set `CHAMBER_TEMP` manually for PLA/PETG.