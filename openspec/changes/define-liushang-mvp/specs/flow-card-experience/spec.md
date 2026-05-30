## ADDED Requirements

### Requirement: Moment-Based Triggering
The system SHALL display a flow-card only after user behavior indicates a meaningful watch moment.

#### Scenario: Meaningful moment is detected
- **WHEN** the user has sufficient dwell time or repeated interactions across videos with overlapping topic or emotion tags
- **THEN** the system marks the moment as triggerable and prepares a flow-card

#### Scenario: Moment is too weak
- **WHEN** the user quickly skips videos without meaningful dwell time or interactions
- **THEN** the system does not display a flow-card

### Requirement: Echo Matching
The system SHALL match the detected moment to one approved echo from the echo library.

#### Scenario: Matching echo exists
- **WHEN** a triggerable moment contains tags that overlap with approved echoes
- **THEN** the system selects one echo with a high match score and returns its public tag and source line

#### Scenario: No suitable echo exists
- **WHEN** no approved echo is suitable for the detected moment
- **THEN** the system does not display a flow-card and records an internal no-match reason

### Requirement: Card Composition
The flow-card MUST center on the matched stranger sentence and avoid direct psychological diagnosis.

#### Scenario: Card is composed
- **WHEN** a matched echo is selected
- **THEN** the card includes a short title, a restrained subtitle, the exact echo text, a source line, and actions for collecting, leaving a sentence, and continuing to scroll

#### Scenario: Card copy is user-facing
- **WHEN** the card is displayed to a user
- **THEN** the card text does not include phrases such as "AI detected your emotion", "I understand your loneliness", or direct claims about the user's identity

### Requirement: Non-Disruptive Appearance
The flow-card SHALL appear as a natural in-feed card rather than a heavy modal interruption.

#### Scenario: Card appears
- **WHEN** a triggerable moment reaches display state
- **THEN** the card visually floats or drifts into the video interface while keeping the user able to dismiss or continue scrolling

### Requirement: Card Actions
The flow-card SHALL support collecting, leaving one sentence, and continuing.

#### Scenario: User collects a card
- **WHEN** the user selects "收下"
- **THEN** the system stores the card in the user's collected flow-cards

#### Scenario: User continues scrolling
- **WHEN** the user selects "继续刷" or scrolls away
- **THEN** the system dismisses the card without requiring contribution

#### Scenario: User chooses to leave a sentence
- **WHEN** the user selects "留一句"
- **THEN** the system opens a short input constrained to a single sentence-length contribution

