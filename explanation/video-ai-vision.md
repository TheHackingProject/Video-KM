---
title: "Video-AI Vision"
type: documentation
diataxis: explanation
status: published
area: video-ai
tags:
  - explanation
  - video-ai
  - vision
  - pedagogy
  - ai
created: 2026-03-12
updated: 2026-03-12
related:
  - "[[reference/video-ai-overview]]"
  - "[[reference/video-lifecycle]]"
  - "[[runbooks/video-ai-development]]"
---

# Video-AI Vision

Explanation: why Video-AI exists and where it is heading (long-term).

## Long-term vision (v1 → v2 → v3)

- **v1 (current)**: Base of pedagogical videos created and maintained by the team. Compositions in Remotion, rendered and integrated into the THP learning experience.
- **v2**: Improvement driven by **user feedback** and **AI-assisted** iteration. Feedback is collected and used to prioritize which videos to refine; tooling and workflows support that loop.
- **v3**: **Guided autonomous regeneration** — AI-assisted updates and regeneration of video content within guardrails defined by the team (quality, pedagogy, alignment with course objectives).

## Video lifecycle (summary)

From idea to production and back: **idea → script → composition → review → render → THP integration → feedback → iteration**. Roles (pedagogy, dev, IA, platform) and where each step lives in the repo are detailed in [reference/video-lifecycle](reference/video-lifecycle.md).

## Pedagogical positioning

Video-AI serves THP’s pedagogical goals: clear, consistent, and maintainable course videos that support web development learning. The structure (intro, concept, code demo, recap) and naming conventions are chosen to keep content discoverable and coherent across modules and lessons.

## Feedback and iteration loop (high-level)

1. Learners and staff consume videos in the THP app.
2. Feedback (explicit or inferred) is stored and linked to video/course identifiers.
3. Devs and content owners use that feedback to decide which videos to improve or regenerate.
4. (Future) AI/skills and tooling help prioritize and draft improvements; humans review and ship.

A dedicated runbook for the feedback loop and AI workflows will be added when those processes are in place (see [runbooks/video-ai-development](runbooks/video-ai-development.md) section 05).

## See also

- [reference/video-ai-overview](reference/video-ai-overview.md) – What Video-AI is and for whom.
- [reference/video-lifecycle](reference/video-lifecycle.md) – Full video lifecycle (steps, roles, where in repo).
- [runbooks/video-ai-development](runbooks/video-ai-development.md) – How to develop and evolve Video-AI day to day.
