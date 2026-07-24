## 1. Core Infrastructure

- [ ] 1.1 Add `COLOR_SCHEME` global variable and palette arrays (`DARK_PALETTE`, `LIGHT_PALETTE`) to `config-lib.sh`
- [ ] 1.2 Add `detect_terminal_background()` function to `config-lib.sh` with OSC query, COLORFGBG, and tput fallbacks
- [ ] 1.3 Add `apply_color_scheme()` function to `config-lib.sh` that selects the palette based on config/flag/auto-detection
- [ ] 1.4 Add `setting.color_scheme` parsing to `load_config()` in `config-lib.sh`
- [ ] 1.5 Add `--color-scheme` CLI flag parsing to `opencode-dockerized.sh` `main()`
- [ ] 1.6 Add `config color-scheme` subcommand to `show_config()` in `opencode-dockerized.sh`

## 2. Script Updates

- [ ] 2.1 Update `opencode-dockerized.sh` to call `apply_color_scheme()` early in `main()`
- [ ] 2.2 Update `run-simple.sh` to call `apply_color_scheme()` before any color output
- [ ] 2.3 Update `setup.sh` to call `apply_color_scheme()` and use configurable colors

## 3. Config & Setup

- [ ] 3.1 Update `init_config_file()` in `config-lib.sh` to include `setting.color_scheme` in the generated config
- [ ] 3.2 Update `save_config()` in `config-lib.sh` to persist `COLOR_SCHEME` setting
- [ ] 3.3 Update `prompt_*` functions in `config-lib.sh` to use the configurable color palette

## 4. Testing & Validation

- [ ] 4.1 Run `bash -n` syntax check on all modified scripts
- [ ] 4.2 Run `shellcheck` on all modified scripts and fix any warnings
- [ ] 4.3 Verify dark palette produces identical output to current behavior
- [ ] 4.4 Verify light palette produces readable output (visually confirm)
- [ ] 4.5 Verify auto-detection works correctly in supported terminals
- [ ] 4.6 Verify auto-detection falls back gracefully in unsupported terminals
