## 1. Project Setup

- [ ] 1.1 Choose and scaffold the frontend stack in `frontend`.
- [ ] 1.2 Choose and scaffold the backend stack in `backend`.
- [ ] 1.3 Add shared demo data shape documentation or TypeScript types for videos, watch events, echoes, cards, and archives.
- [ ] 1.4 Add basic local run scripts for frontend and backend.

## 2. Backend Demo API

- [ ] 2.1 Implement seeded video data with topic tags, emotion tags, summaries, and knowledge nodes.
- [ ] 2.2 Implement seeded echo data with approved status, tags, source type, creation time, and usage count.
- [ ] 2.3 Implement `GET /api/videos`.
- [ ] 2.4 Implement `POST /api/watch-events` to accept and store recent demo watch events.
- [ ] 2.5 Implement simulated moment detection returning `shouldTrigger`, `userMoment`, `matchedTags`, `intensity`, `uiReason`, and `internalReason`.
- [ ] 2.6 Implement echo matching against approved echoes.
- [ ] 2.7 Implement card composition returning title, subtitle, exact echo text, source line, and action labels.
- [ ] 2.8 Implement `POST /api/echoes` with moderation/tagging simulation.
- [ ] 2.9 Implement archive/upstream retrieval for seeded topics.

## 3. Frontend Feed

- [ ] 3.1 Build the first-screen vertical short-video feed.
- [ ] 3.2 Render seeded video metadata, author, title, and interaction controls.
- [ ] 3.3 Implement video navigation and local dwell-time tracking.
- [ ] 3.4 Send watch events to the backend during demo interactions.
- [ ] 3.5 Add a demo/debug mode showing tags, dwell time, watch events, trigger progress, and moment analysis.

## 4. Flow-Card Experience

- [ ] 4.1 Build the flow-card component with restrained copy hierarchy and polished animation.
- [ ] 4.2 Trigger the card only from backend moment/card responses.
- [ ] 4.3 Implement "收下" to store the card in local or backend demo state.
- [ ] 4.4 Implement "继续刷" to dismiss the card without contribution.
- [ ] 4.5 Implement "留一句" modal with length constraints.
- [ ] 4.6 Submit new sentences to the backend moderation/tagging endpoint.

## 5. Upstream And Memory Views

- [ ] 5.1 Build an upstream/archive view for a selected topic or echo.
- [ ] 5.2 Display archive title, short preface, selected echoes, keywords, and sealed status.
- [ ] 5.3 Build a simple "我的" view for collected cards and submitted echoes.

## 6. Product Polish

- [ ] 6.1 Tune seeded videos and echoes so the demo has at least one strong "想起以前" path.
- [ ] 6.2 Ensure card copy avoids direct psychological diagnosis and technical AI wording.
- [ ] 6.3 Tune responsive layout for desktop and mobile viewports.
- [ ] 6.4 Add empty, loading, and error states appropriate for a demo.

## 7. Verification

- [ ] 7.1 Validate OpenSpec artifacts.
- [ ] 7.2 Run frontend lint/build checks.
- [ ] 7.3 Run backend tests or smoke checks.
- [ ] 7.4 Manually verify the full demo path: scroll, trigger card, collect, leave sentence, view upstream, view collected card.

