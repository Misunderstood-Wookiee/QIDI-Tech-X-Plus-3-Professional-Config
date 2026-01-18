# QIDI-Tech X-Plus 3 Professional Config

> **Looking for a better, safer, faster, and modular Klipper config for your X-Plus 3?**
> 
> I was too. That's why I created this enhanced configuration set. Welcome to a more refined printing experience.

## Purpose

This repository contains the **stable and production-ready** Klipper configuration files for the QIDI X-Plus 3 3D printer. It represents a refined, modularized version of the stock configuration with improved organization and maintainability.

## Overview

The stable branch features an enhanced, modular configuration structure compared to the stock branch. Configuration is organized into separate, purpose-driven modules for better maintainability and clarity:

### Configuration Structure

- **Printer.cfg** - Main Klipper configuration file with core printer settings
- **Functionality/** - Feature-specific configuration modules
- **Hardware/** - Hardware-specific configurations and calibrations
- **Macros/** - Custom G-code macros for extended printer functionality

## Modularity Enhancements

The stable configuration demonstrates significant improvements over the stock branch:

- **Separated concerns**: Hardware configurations isolated from functionality modules
- **Macro library**: Custom macros organized for easier management and reuse
- **Maintainability**: Modular design keeps feature updates clean and contained for any custom additions you may wish to make, while improved G‑code documentation gives you clear insight into how everything works—and why it matters.
- **Scalability**: Easier to add new features or modify existing ones without affecting core settings
- **User Friendly**: Slicer-agnostic design philosophy. Simply use `START_PRINT` and `END_PRINT` macros in your slicer of choice—no more hassle with slicer-specific machine G-code scripts when switching slicers. 

## Standout Features
### Adaptive Bed Meshing Improvements
- **Faster Probing**: - speed: 300 vs 150
Higher probing speed reduces leveling time while maintaining accuracy on modern probes.
- **Higher Mesh Resolution**: - probe_count: 7×7 (49 points) vs 6×6 (36 points)
A denser grid captures more detail across the bed surface, improving first‑layer consistency.
- **Stable Z‑Reference**: -  relative_reference_index: 24
Sets the center probe point as the reference height, improving:  Z‑offset consistency
Plate‑swap reliability, Mesh stability
- **Faster Travel Between Probe Points**: - horizontal_move_z: 5 vs 10
Lower Z‑lift reduces unnecessary travel time and mechanical wear.
### General
-  **Chamber Heating is now using PID and can be PID Calibrated**
-  **Speed up homing (40mm/s => 100mm/s)**
- **Improved sensitivity during homing, homes with less noise**
- **Z Homing adjustment for faster movement**
- **Saftey first changes to some Macros to ensure boundry checks are considered**
- **Improved Filament Load and Unload sequence improving reliablity**
- **Manual Bed leveling adjustment**: - is easier thanks to enabled `Screw_Tilt_Adjust` which can be used from the WebUI/Console to assist manual tramming of the build surface before any mesh calibrations.


## Branch Information

This is the `stable` branch, containing production-tested configurations optimized for reliability and performance on the QIDI X-Plus 3 model. 
##### Acknowledgement to https://github.com/qidi-community/config-xplus3 for which this work is derrived.
