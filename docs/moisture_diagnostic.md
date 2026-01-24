# Moisture Diagnostic

## Overview

The `MOISTURE_DIAGNOSTIC` macro is a specialized tool for testing filament moisture levels through controlled extrusion tests. It uses material-aware temperatures via the `_GET_MATERIAL_TEMPS` macro to ensure accurate testing at optimal temperatures for each filament type.

## Features

- **Material-aware heating**: Automatically uses correct extrusion temperature for the specified material
- **Safe parking**: Parks the nozzle before and after testing to prevent accidental collisions
- **Multi-speed testing**: Optional multi-speed diagnostic for comprehensive moisture detection
- **Automatic logging**: Records test results to `/tmp/moisture_check.log` with timestamps
- **User-tunable parameters**: Configurable extrusion lengths, feedrates, and parking positions

## Usage

### Basic Single-Speed Test

```gcode
MOISTURE_DIAGNOSTIC MATERIAL=PLA
```

This performs a single slow extrusion test at the optimal temperature for PLA.

### Multi-Speed Diagnostic

```gcode
MOISTURE_DIAGNOSTIC MATERIAL=ABS MULTI=1
```

This performs three extrusion tests at different speeds (slow, medium, fast) to help identify moisture-related issues at various flow rates.

### Supported Materials

The macro supports all materials configured in your `_GET_MATERIAL_TEMPS` macro:
- PLA
- PETG
- ABS
- ASA
- TPU
- Nylon
- PC (Polycarbonate)

## Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `MATERIAL` | `PLA` | Material type to test (determines extrusion temperature) |
| `MULTI` | `0` | Set to `1` for multi-speed testing, `0` for single-speed |

## Configuration Variables

You can customize the following variables in the macro:

```cfg
variable_extrude_length: 15.0          # Length of filament to extrude per test (mm)
variable_slow_feedrate: 120.0          # Slow extrusion speed (mm/min)
variable_medium_feedrate: 300.0        # Medium extrusion speed (mm/min)
variable_fast_feedrate: 600.0          # Fast extrusion speed (mm/min)
variable_retract: 2.0                  # Retraction distance after each test (mm)
variable_park_x: 10.0                  # X position for parking
variable_park_y: 10.0                  # Y position for parking
variable_park_z: 20.0                  # Z height for parking
variable_logfile: "/tmp/moisture_check.log"  # Log file path
```

## How It Works

1. **Parks the nozzle** at a safe position away from the build area
2. **Retrieves material-specific temperature** using `_GET_MATERIAL_TEMPS`
3. **Heats the extruder** to the appropriate loading temperature
4. **Performs extrusion test(s)**:
   - Single-speed mode: One slow extrusion (best for hearing moisture pops)
   - Multi-speed mode: Three extrusions at different speeds
5. **Logs the results** with timestamp, speed, material, and test parameters
6. **Parks again** to ensure a safe ending position

## Interpreting Results

### What to Listen For

During the extrusion test, listen carefully for:
- **Popping or crackling sounds**: Indicates moisture in the filament
- **Hissing or sizzling**: Suggests significant moisture content
- **Smooth, quiet extrusion**: Indicates dry filament

### Visual Inspection

Observe the extruded filament for:
- **Rough, bubbly surface**: Sign of moisture
- **Inconsistent diameter**: May indicate moisture or other issues
- **Steam or vapor**: Clear sign of excess moisture

### Multi-Speed Testing

When using `MULTI=1`, compare the results at different speeds:
- Moisture issues often become more apparent at slower speeds
- Fast extrusion may mask subtle moisture problems
- Consistent issues across all speeds suggest significant moisture

## Troubleshooting

### Macro Won't Start

- Ensure `RUN_SHELL_COMMAND` is enabled in your Klipper configuration
- Verify that `_GET_MATERIAL_TEMPS` macro is available

### Log File Not Creating

- Check that `/tmp/` directory exists and is writable
- On some systems, you may need to change the log path to a user-writable directory

### Inconsistent Results

- Allow the extruder to fully stabilize at temperature before testing
- Ensure filament path is clear and not binding
- Test the same filament multiple times for consistency

## Best Practices

1. **Test fresh filament**: Establish a baseline for comparison
2. **Regular testing**: Check filament that's been stored for a while
3. **Document results**: Keep track of which spools show moisture issues
4. **Multi-speed comparison**: Use multi-speed mode for comprehensive diagnostics
5. **Listen carefully**: Moisture sounds are often subtle, especially at high speeds

## Integration with Workflow

This macro is designed for standalone use and should be run:
- Before important prints with suspect filament
- After long storage periods
- When troubleshooting print quality issues
- As part of filament maintenance routine

It does **not** interfere with normal printing operations and can be run at any time when the printer is idle.
