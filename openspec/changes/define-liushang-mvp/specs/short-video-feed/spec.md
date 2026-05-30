## ADDED Requirements

### Requirement: Immersive Feed
The system SHALL provide a Web short-video feed as the first product surface.

#### Scenario: User opens the app
- **WHEN** the user opens the MVP app
- **THEN** the system displays a vertical short-video feed without requiring the user to first pass through a marketing landing page

#### Scenario: User navigates videos
- **WHEN** the user scrolls or presses the next control
- **THEN** the system advances to another seeded video and updates the visible video metadata

### Requirement: Seeded Video Metadata
Each video in the feed MUST include structured metadata for topic and emotion analysis.

#### Scenario: Feed loads videos
- **WHEN** the frontend requests videos
- **THEN** each returned video includes an id, title, author, media source or placeholder, topic tags, emotion tags, summary, and optional knowledge nodes

### Requirement: Watch Event Capture
The system SHALL capture demo watch events needed for moment detection.

#### Scenario: User watches a video
- **WHEN** the user stays on a video for a measurable duration
- **THEN** the system records a watch event containing user id, video id, dwell seconds, timestamp, and interaction flags

#### Scenario: User interacts with a video
- **WHEN** the user likes, replays, collects, or opens comments for a video
- **THEN** the next moment analysis includes those interaction flags

### Requirement: Demo Debug Visibility
The feed SHALL support a demo/debug mode that makes the simulated AI process visible.

#### Scenario: Debug mode is enabled
- **WHEN** the presenter enables debug mode
- **THEN** the UI displays current video tags, accumulated dwell time, recent watch events, trigger progress, and matched moment data

#### Scenario: Debug mode is disabled
- **WHEN** debug mode is disabled
- **THEN** the feed hides technical labels and preserves the polished user-facing experience

