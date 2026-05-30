## Why

「流觞」需要在赛道一“刷到懂你的瞬间”中保持足够锋利：核心不是做一个功能丰富的社区，而是在短视频信息流里创造一个让用户停下来的 AI 卡片体验。

当前项目已有产品文档和提示词雏形，但需要用 OpenSpec 固化 MVP 边界、体验契约和实现任务，避免后续开发发散到好友、群聊、知识库或完整社区。

## What Changes

- Introduce a Web short-video feed that can simulate scrolling, watching, liking, replaying, and opening comments.
- Introduce AI-assisted moment detection based on video tags and user watch behavior.
- Introduce a reusable echo library containing short human-written phrases with topic and emotion tags.
- Introduce a flow-card experience that appears only after a meaningful watch threshold is reached.
- Introduce a lightweight "leave one sentence" interaction that adds a moderated/tagged echo for later users.
- Introduce an upstream/archive view that shows where an echo came from and how a topic can be sealed.
- Provide demo-friendly simulated AI first, with clear seams for replacing rules with model calls later.
- Explicitly exclude real group chat, friend relationship graphs, full recommendation systems, and large-scale moderation from the MVP.

## Capabilities

### New Capabilities

- `short-video-feed`: Covers the immersive Web feed, video metadata, user watch events, and demo interactions.
- `flow-card-experience`: Covers moment detection, echo matching, card composition, card actions, and card display constraints.
- `echo-library`: Covers seeded echoes, user-submitted echoes, moderation/tagging, and archive/upstream views.
- `ai-skill-pipeline`: Covers prompt/skill boundaries for video understanding, moment detection, echo matching, card composition, moderation, and archive curation.

### Modified Capabilities

- None.

## Impact

- Frontend: `frontend` will need a short-video feed, flow-card component, leave-one-sentence modal, upstream/archive page, user memory page, and demo/debug panel.
- Backend: `backend` will need APIs for videos, watch events, moment analysis, card matching, echo submission, and archive retrieval.
- Data: Demo seed data should include videos, echoes, topics, watch events, and archived flow examples.
- AI: Initial implementation may use deterministic rules and seeded content, but interfaces should mirror the future AI pipeline.
- Product docs: Existing documents under `docs/` remain the narrative source; OpenSpec becomes the implementation constraint source.
