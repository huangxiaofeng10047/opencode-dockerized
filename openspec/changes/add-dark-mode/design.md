## Context

The project uses ANSI escape codes for colored terminal output across multiple shell scripts (`opencode-dockerized.sh`, `config-lib.sh`, `run-simple.sh`, `setup.sh`). Currently colors are hardcoded to bright variants (RED, GREEN, YELLOW, BLUE) that assume a dark terminal background. Users with light terminals report poor legibility, especially for yellow and blue.

The color definitions live in two places:
- `opencode-dockerized.sh` (lines 14-18): defines RED, GREEN, YELLOW, BLUE, NC
- `config-lib.sh` (lines 21-25): defines fallback defaults using `: "${VAR:=value}"` pattern

Only `config-lib.sh` has the `config_info`/`config_success`/`config_warning`/`config_error` wrapper functions. The main script and `setup.sh` call the lower-level `print_*` functions directly.

## Goals / Non-Goals

**Goals:**
- Users can choose between dark-optimized and light-optimized color palettes
- Automatic detection of terminal background when color_scheme=auto
- All color output respects the chosen scheme
- Backward compatible — existing configs default to `dark` behavior

**Non-Goals:**
- Full 16/256-color theme customization (no custom per-color overrides)
- GUI or interactive color picker
- Changing OpenCode's own color scheme inside the container

## Decisions

- **Detection via `tput` and `COLORFGBG`**: Use `tput setab` fallback chain, then `$COLORFGBG` environment variable, then `terminal-background-color` OSC query (with timeout). Fall back to `dark` if detection fails. This provides reliable detection across most terminals without external dependencies.
- **Separate palette arrays per mode**: Define two associative arrays (`DARK_PALETTE` and `LIGHT_PALETTE`) mapping role names to ANSI codes. A computed `COLOR_PALETTE` array is selected at init. This avoids scattering conditional checks throughout the code.
- **Config key `setting.color_scheme`**: Values `dark`, `light`, `auto`. Parsed in `load_config()` alongside existing settings like `ssh_agent_support`. This keeps the INI format consistent.
- **Runtime flag `--color-scheme`**: Override config at invocation. Applied early in `main()` before any color output.
- **Existing behavior as default**: If no setting and no flag, behave as before (`dark` palette). Detection-only path is opt-in via `auto`.

## Risks / Trade-offs

- [False positive detection] → If `auto` mode misdetects the background, colors may be wrong. Mitigation: `auto` falls back to `dark`, and users can explicitly set `light` or `dark` in config.
- [Unsupported terminals] → Some terminals may not support background detection. Mitigation: silent fallback to `dark` with no error output.
- [OSC query parsing] → The `terminal-background-color` sequence requires reading from stdin briefly. Mitigation: use timeout-based read (1 second) to avoid hanging.
