---
title: "Component Shortlist (Remotion)"
type: documentation
diataxis: reference
status: published
area: video-ai
tags:
  - video-ai
  - components
  - remotion-lib
created: 2026-03-12
updated: 2026-03-12
related:
  - "[[video-ai-preparation/01-video-formats]]"
  - "[[runbooks/video-ai-development]]"
  - "[[00-architecture]]"
---

# Component Shortlist (Remotion)

Derived from [01-video-formats](01-video-formats.md). Implement in `packages/remotion-lib` when ready. **Build P0 first for the first pilot** (3–4 components); do not tackle many in parallel.

---

## Legos (primitives)

| Component | Priority | Notes |
|-----------|----------|--------|
| TitleCard | P0 | Title + optional subtitle; used in both formats. |
| SectionIntro | P0 | Short intro line or hook for a section. |
| CodeBlockWithHighlight | P0 | Code + line-by-line highlight; core for Format 2. |
| BulletList | P1 | Simple bullet list for concept slides. |
| Callout | P1 | Highlight box or callout for key points. |
| ProgressBar | P1 | Step or progress indicator for code walkthrough. |

---

## Pedagogical patterns

| Component | Priority | Notes |
|-----------|----------|--------|
| ConceptSlide | P0 | Orchestrates title + text/bullets + optional callout; for Format 1. |
| CodeAlongStep | P0 | One step in a code demo: explanation + code highlight; for Format 2. |
| QuizSlide | P1 | Question + options (future); not required for first pilot. |

---

## Implementation order

1. Implement P0 legos: TitleCard, SectionIntro, CodeBlockWithHighlight.
2. Implement P0 patterns: ConceptSlide, CodeAlongStep (using the P0 legos).
3. Use them in 1–2 pilot compositions in `apps/remotion` before adding P1 components.
