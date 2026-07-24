## ADDED Requirements

### Requirement: System detects terminal background in auto mode

When `setting.color_scheme=auto`, the system SHALL attempt to detect the terminal background color and select the appropriate palette.

#### Scenario: Auto mode detects dark background
- **WHEN** `setting.color_scheme=auto` and the terminal reports a dark background
- **THEN** the system SHALL select the `dark` palette

#### Scenario: Auto mode detects light background
- **WHEN** `setting.color_scheme=auto` and the terminal reports a light background
- **THEN** the system SHALL select the `light` palette

### Requirement: Detection uses multiple fallback strategies

The system SHALL try detection strategies in order: OSC query via `terminal-background-color`, `$COLORFGBG` environment variable, then `tput` with terminal capability database. If all fail, it SHALL fall back to `dark`.

#### Scenario: Detection via COLORFGBG
- **WHEN** `$COLORFGBG` ends with `;0` (black background)
- **THEN** the system SHALL select the `dark` palette without trying further detection methods

#### Scenario: Detection via COLORFGBG light
- **WHEN** `$COLORFGBG` ends with `;7` or `;15` (white background)
- **THEN** the system SHALL select the `light` palette without trying further detection methods

#### Scenario: Detection via OSC query
- **WHEN** `$COLORFGBG` is not set and the terminal responds to the `\e]11;?\a` OSC sequence with an RGB value
- **THEN** the system SHALL compute luminance from the RGB value and select `dark` if luminance <= 0.5, `light` otherwise

#### Scenario: All detection methods fail
- **WHEN** `$COLORFGBG` is not set and the OSC query times out and `tput` cannot determine the background
- **THEN** the system SHALL silently fall back to the `dark` palette

### Requirement: Detection does not hang or output garbage

The OSC query SHALL use a read timeout (max 1 second) to prevent hanging. Any unexpected terminal output during detection SHALL be discarded.

#### Scenario: OSC query timeout
- **WHEN** the terminal does not respond to the `\e]11;?\a` sequence within 1 second
- **THEN** the system SHALL proceed to the next fallback strategy without printing any error
