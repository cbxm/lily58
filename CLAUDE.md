# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a ZMK (Zephyr Mechanical Keyboard) firmware configuration for a Lily58 split keyboard using nice!nano v2 controllers. The project uses the ZMK framework built on Zephyr RTOS for wireless mechanical keyboards.

## Build Commands

### Build Individual Keyboard Halves
```bash
# Build left half
west build -d build/left -b nice_nano_v2 -- -DSHIELD=lily58_left

# Build right half  
west build -d build/right -b nice_nano_v2 -- -DSHIELD=lily58_right
```

### Pristine Build (Clean Build)
```bash
# Left half with clean build
west build -p -d build/left -b nice_nano_v2 -- -DSHIELD=lily58_left

# Right half with clean build
west build -p -d build/right -b nice_nano_v2 -- -DSHIELD=lily58_right
```

The `-p` flag performs a pristine build, clearing the build cache. The `-d build/left` and `-d build/right` flags create separate build directories for each half to avoid overwriting firmware files.

## Project Structure

```
lily58/
├── build.yaml          # GitHub Actions build matrix configuration
├── config/
│   ├── lily58.conf     # ZMK configuration settings (power, BT, etc.)
│   ├── lily58.keymap   # Main keymap definition file
│   └── west.yml        # West workspace configuration
└── .github/workflows/
    └── build.yml       # GitHub Actions workflow for automated builds
```

## Keymap Architecture

The keymap is defined in `config/lily58.keymap` and follows ZMK's device tree structure:

### Key Components
- **Behaviors**: Custom key behaviors including homerow mods, tap-dances, and hold-taps
- **Macros**: Custom macros like em-dash generation
- **Combos**: Key combinations that trigger special actions
- **Layers**: Multiple keyboard layers for different functions
- **Conditional Layers**: Layers that activate when multiple layers are held

### Layer Structure (0-indexed)
- Layer 0: `base` - Default QWERTY layout with homerow mods
- Layer 1: `NumLeft` - Number pad on left side  
- Layer 2: `NumRight` - Number pad on right side
- Layer 3: `FkeysLeft` - Function keys on left side
- Layer 4: `FkeysRight` - Function keys on right side
- Layer 5: `symbolsLeft` - Symbols and punctuation on left
- Layer 6: `symbolsRight` - Symbols and punctuation on right
- Layer 7: `NavLeft` - Navigation keys on left side
- Layer 8: `NavRight` - Navigation keys on right side
- Layer 9: `boardLeft` - Keyboard control (BT, media) on left
- Layer 10: `boardRight` - Keyboard control (BT, media) on right

### Custom Behaviors
- `hm`: Homerow mods with 200ms tapping term
- `dash_dance`: Hold for em-dash, tap for regular key
- `LayerModTap`: Hold for momentary layer, tap for toggle

## Hardware Configuration

The keyboard uses:
- nice!nano v2 controllers for both halves
- nice_view displays with adapters
- ZMK Studio enabled for real-time keymap updates
- Power management optimized for wireless usage
- Bluetooth transmission power set to +8dBm

## Development Notes

- Configuration changes require rebuilding and flashing firmware to both halves
- ZMK Studio allows real-time keymap changes without rebuilding
- Power and Bluetooth settings in `lily58.conf` affect battery life and connectivity
- Combo key positions refer to the physical key matrix positions
- Layer numbers in `&lt`, `&mo`, and `&tog` behaviors correspond to the layer definitions