## Why

The current output color scheme uses hardcoded ANSI colors (bright red, green, yellow, blue) that may be illegible on light terminal backgrounds. Users with light-themed terminals often cannot read the yellow warning text or blue info text. There is no way to customize or switch the color scheme.

## What Changes

- Add a `setting.color_scheme` config option (`dark`, `light`, `auto`)
- Define two color palettes: one optimized for dark backgrounds (current default), one for light backgrounds
- Add terminal background detection for `auto` mode (using `terminal-background-color` or `tput` / `COLORFGBG`)
- Update all scripts using color output (`opencode-dockerized.sh`, `config-lib.sh`, `run-simple.sh`, `setup.sh`, `entrypoint.sh`) to use the configurable scheme
- Add a `--color-scheme` CLI flag to override the config setting at runtime
- Add `config color-scheme` subcommand to show/switch schemes

## Capabilities

### New Capabilities
- `color-scheme-config`: Configurable color themes via user config and CLI flag, with dark, light, and auto-detect modes
- `terminal-background-detection`: Automatic detection of terminal light/dark background for optimal default color selection

### Modified Capabilities

- (none — this is a new feature)

## Impact

- **Scripts modified**: `opencode-dockerized.sh`, `config-lib.sh`, `run-simple.sh`, `setup.sh`
- **Config file**: New `setting.color_scheme` key added to the INI config
- **Backward compatibility**: Existing configs without `setting.color_scheme` default to `dark` (unchanged behavior)
- **No Dockerfile changes needed** — all changes are in shell scripts
