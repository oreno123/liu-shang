## Context

「流觞」 is a competition MVP for the "AI experience: the moment you feel understood while scrolling" track. The product must look and feel like a short-video feed, but its core differentiator is the flow-card moment: AI matches the user's current watch state with a past stranger's sentence.

The repository currently contains `backend`, `frontend`, and product docs under `docs/`. No application implementation exists yet. The MVP must therefore establish a stable architecture, seed data shape, API surface, and UI constraints before coding begins.

## Goals / Non-Goals

**Goals:**

- Build a demo-ready Web short-video feed where users can scroll through seeded videos.
- Detect meaningful watch moments using deterministic rules that mirror future AI behavior.
- Display a polished flow-card only when the moment threshold is reached.
- Let users collect a card or leave one short sentence for later users.
- Preserve a clear AI pipeline boundary so simulated rules can later be replaced by model calls.
- Keep the experience emotionally restrained and centered on "a past sentence arriving now."

**Non-Goals:**

- Real Douyin/TikTok integration.
- Real-time group chat or direct messaging.
- Friend recommendation, relationship graphs, or social matching.
- Full-scale moderation operations.
- Production-grade recommendation ranking.
- Full visual-search track implementation.

## Decisions

### Decision: Use seeded demo data first

Videos, echoes, topics, and archive examples will be seeded locally.

Rationale: The competition demo needs deterministic pacing and emotional quality. Real APIs or fully dynamic AI generation would add cost, latency, and instability before the core experience is proven.

Alternative considered: Generate every card live with a model. This was rejected for MVP because it risks inconsistent tone and makes demos harder to control.

### Decision: Simulate AI through replaceable service boundaries

The backend will expose AI-shaped services:

- video understanding
- moment detection
- echo matching
- card composition
- echo moderation/tagging
- archive curation

Initial implementations can use rules and seed tags, but their inputs/outputs should match the prompt contracts in `docs/02-ai-skill-prompts.md`.

Rationale: This keeps the MVP cheap and stable while preserving a credible path to real model integration.

Alternative considered: Put all matching logic in the frontend. This was rejected because it would blur future AI integration boundaries and make API demos less convincing.

### Decision: Treat flow-card as the primary product surface

The flow-card must be the dominant interaction after it appears. It should not compete with chat panels, friend lists, knowledge cards, or dense dashboards.

Rationale: The competition prompt rewards the moment of stopping. A crowded interface would dilute the emotional hit.

Alternative considered: Add a richer community panel with same-interest users. This remains a future extension, not MVP.

### Decision: Use "leave one sentence" instead of comments or groups

User contribution is limited to a short sentence for later viewers. It becomes an echo after moderation and tagging.

Rationale: This preserves the cross-time "upstream/later viewer" metaphor while avoiding the operational weight of a social network.

Alternative considered: Threaded comments under each flow-card. This was rejected for MVP because it collapses the product back into a comment section.

### Decision: Include a demo/debug mode

The frontend should support a visible demo/debug panel showing current tags, dwell time, matched moment, and trigger progress.

Rationale: The final product should feel poetic, but judges also need to see the AI logic. A toggleable debug view lets the demo explain itself without polluting the normal experience.

Alternative considered: Always show AI reasoning in the card. This was rejected because terms like "AI detected your emotion" are emotionally clumsy.

## Risks / Trade-offs

- [Risk] Seeded content feels fake or over-written → Mitigation: keep echo sentences concrete, short, and restrained; avoid direct psychological claims.
- [Risk] The product is mistaken for a comment feature → Mitigation: copy and UI must emphasize "upstream", "later viewer", and cross-time matching instead of discussion.
- [Risk] Moment detection feels arbitrary → Mitigation: demo/debug panel must show tags, watch events, and trigger threshold clearly.
- [Risk] Scope expands into social networking → Mitigation: specs explicitly exclude group chat, direct messaging, and friend graphs from MVP.
- [Risk] Model-generated text becomes oily → Mitigation: prompt constraints forbid diagnosis, forced warmth, and chicken-soup phrasing.

