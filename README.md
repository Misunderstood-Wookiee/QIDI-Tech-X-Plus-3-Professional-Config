![Status](https://img.shields.io/badge/status-active-brightgreen)
![Klipper](https://img.shields.io/badge/klipper-compatible-blue)
![Qidi X‑Plus 3](https://img.shields.io/badge/Qidi-X--Plus%203-orange)
![License: MIT](https://img.shields.io/badge/license-MIT-yellow)
![Docs](https://img.shields.io/badge/docs-online-blueviolet)

# QIDI-Tech X-Plus 3 Professional Config

> **Looking for a better, safer, faster, and modular Klipper config for your X-Plus 3?**
> 
> I was too. That's why I created this enhanced configuration set. Welcome to a more refined printing experience.

## Purpose

This repository contains the **stable and production-ready** Klipper configuration files for the QIDI X-Plus 3 3D printer. It represents a refined, modularized version of the stock configuration with improved organization and maintainability.

## Overview

The stable branch features an enhanced, modular configuration structure compared to the stock branch. Configuration is organized into separate, purpose-driven modules for better maintainability and clarity: in-part https://github.com/qidi-community/config-xplus3 for which this work is derrived.

### Configuration Structure

- **Printer.cfg** - Main Klipper configuration file with core printer settings
- **Functionality/** - Feature-specific configuration modules
- **Hardware/** - Hardware-specific configurations and calibrations
- **Macros/** - Custom G-code macros for extended printer functionality

## Modularity
The stable configuration demonstrates significant improvements over the stock branch:

- **Separated concerns**: Hardware configurations isolated from functionality modules
- **Macro library**: Custom macros organized for easier management and reuse
- **Maintainability**: Modular design keeps feature updates clean and contained for any custom additions you may wish to make, while improved G‑code documentation gives you clear insight into how everything works—and why it matters.
- **Scalability**: Easier to add new features or modify existing ones without affecting core settings
- **User Friendly**: Slicer-agnostic design philosophy. Simply use `PRINT_START` and `END_PRINT` macros in your slicer of choice—no more hassle with slicer-specific machine G-code scripts when switching slicers.

## ✨ Features

### 🔧 PRINT_START Enhancements
- Adaptive purge length based on first‑layer height  
- Material‑aware chamber logic  
  - ABS/ASA → chamber defaults to 45°C  
  - PLA/PETG → chamber disabled unless explicitly set  
- Optional Z‑offset compensation via `Z_ADJUST`  
- Thermal‑expansion‑aware sequencing  
- Nozzle heats last to prevent ooze  
- Integrated SMART_PARK before purge  
- No slicer overrides required  

### 🎯 SMART_PARK
- Safe, consistent parking position  
- Avoids clips, front airflow, and bed contamination  
- Used by PRINT_START, PAUSE, RESUME, and CANCEL_PRINT  

### 🧼 PRINT_END
- Safe Z‑lift  
- SMART_PARK  
- Heaters and fans off  
- Mesh cleared  
- Motors disabled  
- Idle timeout restored  

### 🛑 PAUSE / ▶️ RESUME (Final Qidi‑Safe Implementation)

This macro suite now uses the correct flow:
The Qidi X‑Plus 3 uses a modified Klipper fork.  
The correct resume primitive is: BASE_RESUME

#### PAUSE
- Saves only the park state (`PAUSEPARK`)
- Lets Klipper/Qidi save the true resume point internally
- Performs a safe Z‑hop
- Parks the toolhead
- Cools the nozzle
- Prevents auto‑resume

#### RESUME
- Reheats to the stored temperature
- Restores from `PAUSEPARK` (leaves the park position cleanly)
- Performs a single prime
- Calls `BASE_RESUME` to:
  - restore the original paused XY/Z
  - resume the G‑code stream

This ensures:
- no double‑movement  
- no double‑prime  
- no double Z‑hop  
- no stalling  
- perfect resume behavior from the Qidi LCD or G‑code    

### ❌ CANCEL_PRINT
- Safe Z‑lift  
- SMART_PARK  
- Heaters and fans off  
- Mesh cleared  
- SD file reset  
- No PRINT_END duplication  
- No Qidi auto‑resume behavior  

### 🔄 Filament Management
- FILAMENT_LOAD: staged fast/slow loading  
- FILAMENT_UNLOAD: staged slow/fast unloading  
- User‑tunable lengths  

### 🧪 LINE_PURGE Integration
- PRINT_START configures purge length via `SETUP_LINE_PURGE`  
- Fully compatible with KAMP‑style adaptive purge macros  

---

## 📂 Macro Overview

| Macro | Purpose |
|-------|---------|
| `PRINT_START` | Full start‑of‑print automation with adaptive logic |
| `PRINT_END` | Clean shutdown and safe parking |
| `PAUSE` | Safe Z‑hop, park, cooldown, and state save |
| `RESUME` | Clean restore, prime, and resume |
| `CANCEL_PRINT` | Safe cancel without double‑execution |
| `SMART_PARK` | Unified safe parking routine |
| `FILAMENT_LOAD` | Staged filament loading |
| `FILAMENT_UNLOAD` | Staged filament unloading |
| `LINE_PURGE` | Adaptive purge line (external macro) |

---

## 🛠 Requirements

- Qidi X‑Plus 3 running Klipper  
- OrcaSlicer or compatible slicer  
- Optional: KAMP‑style adaptive purge macro  

---

## 📜 License

GNU GENERAL PUBLIC LICENSE — free to use, modify, and contribute.

---

## 🤝 Contributing

Pull requests are welcome.  
Please ensure changes are:

- Qidi‑safe  
- Consistent with the macro style  
- Documented in the CHANGELOG  

---

## 📣 Support

Open an issue on GitHub with:

- Klipper logs  
- Macro snippets  
- Steps to reproduce  

This helps keep the project stable and contributor‑friendly.


## Branch Information
This is the `stable beta testing` branch, containing production-tested configurations optimised with observed normal operation reliability (however not final) and performance on the QIDI X-Plus 3 model. 