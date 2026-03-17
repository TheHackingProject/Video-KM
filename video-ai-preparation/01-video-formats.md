---
title: "Video Formats (Pedagogical)"
type: documentation
diataxis: reference
status: published
area: video-ai
tags:
  - video-ai
  - formats
  - pedagogy
created: 2026-03-12
updated: 2026-03-12
related:
  - "[[reference/video-ai-overview]]"
  - "[[explanation/video-ai-vision]]"
  - "[[runbooks/video-ai-development]]"
---

# Video Formats (Pedagogical)

Definition of the 1–2 pedagogical video formats used for THP course videos. Each format has a pedagogical goal, target duration, structure, and visual needs. Use these to derive the component shortlist before writing Remotion code.

---

## Format 1 – Concept intro (short)

- **Pedagogical goal**: Introduce a single concept in under 1 minute so learners are not overwhelmed; one idea per clip.
- **Target duration**: 30–60 s.
- **Structure**: intro (hook) → concept (one idea) → optional micro-recap.
- **Visual needs**: title card, simple text or bullet list, minimal animation, optional callout.

---

## Format 2 – Code demo guided

- **Pedagogical goal**: Walk through code step-by-step so learners can follow and replay at their own pace.
- **Target duration**: 2–5 min (to be set per lesson).
- **Structure**: intro → concept reminder → code walkthrough (explanation + code + line-by-line highlight) → recap.
- **Visual needs**: title/section titles, code block with highlight, callouts, progress or step indicator, transitions between steps.

---

## Next step

Derive the component shortlist from these formats in [02-component-shortlist](02-component-shortlist.md), then implement P0 components in `packages/remotion-lib` before building compositions in `apps/remotion`.
