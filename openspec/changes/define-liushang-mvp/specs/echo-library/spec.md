## ADDED Requirements

### Requirement: Seeded Echo Library
The system SHALL provide an initial library of approved echoes for demo matching.

#### Scenario: Echo library is loaded
- **WHEN** the backend starts or seed data is loaded
- **THEN** the system has approved echoes with text, topic tags, emotion tags, source type, creation time, status, and usage count

### Requirement: Echo Quality Constraints
Echo text MUST be short, concrete, and suitable for appearing as a stranger's past sentence.

#### Scenario: Echo is eligible for matching
- **WHEN** an echo is available for card matching
- **THEN** it is approved, does not contain advertising or unsafe content, and does not directly diagnose a user's emotional state

### Requirement: Leave One Sentence
The system SHALL allow users to submit one short sentence for later viewers.

#### Scenario: User submits a valid sentence
- **WHEN** the user submits a sentence within the configured length limit
- **THEN** the system moderates it, tags it, stores it as an approved or pending echo, and makes it eligible according to moderation status

#### Scenario: User submits unsafe or invalid content
- **WHEN** the submitted sentence contains unsafe, advertising, private, or abusive content
- **THEN** the system rejects it or stores it as not approved and does not use it for matching

### Requirement: Upstream View
The system SHALL provide an upstream view for a topic or selected echo.

#### Scenario: User opens upstream
- **WHEN** the user opens the upstream view from a flow-card or navigation
- **THEN** the system displays the topic, selected echo, related approved echoes, representative tags, and archive status

### Requirement: Archive View
The system SHALL support a sealed archive representation for a topic.

#### Scenario: Topic is archived
- **WHEN** a topic is presented as sealed for the demo
- **THEN** the archive view displays a title, short preface, selected echoes, and keywords

