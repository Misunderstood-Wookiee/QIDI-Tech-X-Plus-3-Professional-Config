## Chamber Lighting System Documentation

The X‑Plus 3 includes a chamber LED strip connected to PC7. This lighting system is integrated into
the printer’s workflow to provide visual feedback during printing, filament handling, and error
states.

### Features:
- Automatic light-on during PRINT_START
- Automatic light-off with completion blink during PRINT_END
- Visual cues during filament load/unload
- Blink patterns for pause, cancel, and error states
- Manual control via LIGHT_ON, LIGHT_OFF, LIGHT_TOGGLE
- LIGHT_BLINK macro for custom signaling

This system improves usability, visibility, and workflow awareness without requiring any hardware
modifications.