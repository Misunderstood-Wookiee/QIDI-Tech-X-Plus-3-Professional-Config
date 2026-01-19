# Maintainer Guide

This document provides guidance for maintainers of the Qidi X‑Plus 3 Macro Suite.

---

## 🔧 Responsibilities

- Review PRs for safety and consistency  
- Ensure Qidi‑safe behavior  
- Maintain documentation  
- Keep CHANGELOG up to date  
- Validate macros against Klipper syntax  

---

## 🧩 Macro Review Checklist

### General
- [ ] No duplicated logic  
- [ ] No unnecessary `_BASE_*` calls  
- [ ] No slicer‑dependent hacks  
- [ ] No hard‑coded machine values unless safe  

### PAUSE/RESUME
- [ ] PAUSE stores `zhop` and `etemp`  
- [ ] RESUME restores state once  
- [ ] No auto‑resume behavior  
- [ ] No double‑restore
- [ ] RESUME ends with `BASE_RESUME` (not `_BASE_RESUME`, `RESUME_BASE`, or `RESUME`)
- [ ] PAUSE does not save its own resume state; relies on Klipper/Qidi internal pause state
- [ ] Only `PAUSEPARK` is restored manually  

### PRINT_START
- [ ] Adaptive purge logic intact  
- [ ] Chamber logic correct  
- [ ] Z‑offset applied after probing  
- [ ] SMART_PARK used  

### CANCEL_PRINT
- [ ] No PRINT_END duplication  
- [ ] All heaters and fans shut down  

---

## 🧪 Testing Matrix

Test each macro under:

| Scenario | Expected Behavior |
|---------|-------------------|
| PLA | Chamber off unless set |
| ABS | Chamber defaults to 45°C |
| First layer 0.08 | Short purge |
| First layer 0.32 | Long purge |
| Pause at low Z | Safe Z‑hop |
| Pause at max Z | Z‑hop disabled |
| Resume after long pause | Reheat + clean restore |
| Cancel mid‑print | Safe shutdown |

---

## 📦 Release Process

1. Update version in `CHANGELOG.md`  
2. Tag release  
3. Publish release notes  
4. Verify documentation builds  