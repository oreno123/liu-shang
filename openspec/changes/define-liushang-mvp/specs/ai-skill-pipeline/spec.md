## ADDED Requirements

### Requirement: AI Pipeline Boundaries
The system SHALL keep AI responsibilities behind explicit service boundaries.

#### Scenario: Moment pipeline runs
- **WHEN** a watch event is submitted
- **THEN** the backend can process it through video understanding, moment detection, echo matching, and card composition boundaries

### Requirement: Simulated AI Compatibility
The initial rule-based implementation MUST use input and output shapes compatible with the documented AI prompts.

#### Scenario: Rule service returns moment analysis
- **WHEN** the simulated moment detector returns a result
- **THEN** the response includes shouldTrigger, userMoment, matchedTags, intensity, uiReason, and internalReason

#### Scenario: Rule service returns card composition
- **WHEN** the simulated card composer returns a card
- **THEN** the response includes title, subtitle, echoText, sourceLine, and action labels

### Requirement: Prompt Source of Truth
The project SHALL keep AI prompt contracts documented and versionable.

#### Scenario: Developer updates AI behavior
- **WHEN** AI behavior changes beyond implementation details
- **THEN** the relevant prompt contract or OpenSpec requirement is updated before or alongside implementation

### Requirement: Moderation Boundary
User-submitted echoes MUST pass through a moderation/tagging boundary before becoming eligible for matching.

#### Scenario: Echo is submitted
- **WHEN** a user leaves a sentence
- **THEN** the system evaluates approval, clean text, topic tags, emotion tags, risk flags, and rejection reason before matching can use it

