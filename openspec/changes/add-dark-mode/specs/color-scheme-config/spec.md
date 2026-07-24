## ADDED Requirements

### Requirement: User can configure color scheme via config file

The system SHALL support a `setting.color_scheme` key in the INI config file with values `dark`, `light`, or `auto`. The default SHALL be `dark` when the key is absent.

#### Scenario: Config key set to dark
- **WHEN** the config file contains `setting.color_scheme=dark`
- **THEN** the system SHALL use the dark background color palette

#### Scenario: Config key set to light
- **WHEN** the config file contains `setting.color_scheme=light`
- **THEN** the system SHALL use the light background color palette

#### Scenario: Config key set to auto
- **WHEN** the config file contains `setting.color_scheme=auto`
- **THEN** the system SHALL attempt to detect the terminal background and select the appropriate palette

#### Scenario: Config key absent
- **WHEN** the config file does not contain `setting.color_scheme`
- **THEN** the system SHALL default to the `dark` palette

### Requirement: User can override color scheme via CLI flag

The system SHALL support a `--color-scheme` flag on the CLI with values `dark` or `light`. When provided, this SHALL override the config file value.

#### Scenario: CLI flag overrides config
- **WHEN** the config file has `setting.color_scheme=light` and the user runs `opencode-dockerized.sh --color-scheme dark run`
- **THEN** the system SHALL use the dark palette

#### Scenario: CLI flag with auto config
- **WHEN** the config file has `setting.color_scheme=auto` and the user runs `opencode-dockerized.sh --color-scheme light run`
- **THEN** the system SHALL use the light palette

### Requirement: Dark palette provides readable colors on dark backgrounds

The dark palette SHALL define ANSI color codes optimized for visibility on dark (black) terminal backgrounds.

#### Scenario: Dark palette color values
- **WHEN** the color scheme is `dark`
- **THEN** RED SHALL be bright red `\033[0;31m`, GREEN SHALL be bright green `\033[0;32m`, YELLOW SHALL be bright yellow `\033[1;33m`, BLUE SHALL be bright blue `\033[0;34m`

### Requirement: Light palette provides readable colors on light backgrounds

The light palette SHALL define ANSI color codes optimized for visibility on light (white) terminal backgrounds.

#### Scenario: Light palette color values
- **WHEN** the color scheme is `light`
- **THEN** RED SHALL be dark red `\033[0;31m` (unchanged), GREEN SHALL be dark green `\033[0;32m` (unchanged), YELLOW SHALL be bold brown `\033[0;33m`, BLUE SHALL be bold blue `\033[1;34m`

### Requirement: System prints current color scheme

The system SHALL support a `config color-scheme` subcommand that displays the currently active color scheme and palette in use.

#### Scenario: Show color scheme
- **WHEN** the user runs `opencode-dockerized.sh config color-scheme`
- **THEN** the system SHALL print the active scheme name (`dark`, `light`, or `auto`) and the detected mode if in `auto`
