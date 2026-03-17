---
title: "Video-AI Development Runbook"
type: runbook
diataxis: how-to
status: published
area: video-ai
tags:
  - runbook
  - video-ai
  - development
  - workflow
created: 2026-03-12
updated: 2026-03-12
related:
  - "[[00-architecture]]"
  - "[[reference/video-ai-overview]]"
  - "[[reference/video-lifecycle]]"
  - "[[explanation/video-ai-vision]]"
  - "[[runbooks/remotion]]"
---

# Video-AI Development Runbook

Single operational entry point for developing and evolving Video-AI: purpose, context, workflow, conventions, quality, and deployment. For rendering/ops (e.g. Lambda, batch render), see a future runbook: `video-ai-rendering.md`.

---

## 01 – Project purpose and scope

Video-AI produces and evolves **pedagogical web development videos** for The Hacking Project (THP).

- **v1**: Team-created base of videos; compositions in this repo, rendered and integrated into the THP app.
- **v2**: Feedback and AI-driven improvement of existing videos.
- **v3**: Guided autonomous regeneration of content within team-defined guardrails.

For full scope and audience, see [reference/video-ai-overview](reference/video-ai-overview.md). For long-term vision and v1/v2/v3, see [explanation/video-ai-vision](explanation/video-ai-vision.md).

---

## 02 – Key context

- **Video lifecycle**: From idea to prod and back: idea → script → composition → review → render → THP integration → feedback → iteration. Full detail (steps, who does what, where in repo): [reference/video-lifecycle](reference/video-lifecycle.md).
- **Monorepo**: Turborepo; apps live under `apps/` (e.g. `apps/remotion`), shared packages under `packages/` (e.g. `packages/remotion-lib`, `packages/ui`).
- **Remotion**: Used to author video compositions. Compositions are registered in `apps/remotion/src/remotion/Root.tsx` and live under `apps/remotion/src/remotion/compositions/`. Rendered outputs are consumed by the THP app.
- **Three levels**: Static UI (`packages/ui`), animated building blocks (`packages/remotion-lib`, and currently `packages/ui/src/lib/remotion`), and compositions (`apps/remotion`). See [00-architecture](00-architecture.md) and [runbooks/remotion](remotion.md).

---

## 03 – Development workflow

**Goal**: Add or edit a THP course video.

1. **Clone and install**  
   From repo root: `git clone --recurse-submodules <repo-url>`, then `bun install`.

2. **Run dev**  
   `bun run dev --filter=remotion` (or `cd apps/remotion && bun run dev`) to open Remotion Studio and preview compositions.

3. **Where to work**  
   - **Compositions** (full video “scenes”): create or edit under `apps/remotion/src/remotion/compositions/` (e.g. `demos/`, or domain folders like `marketing/`, `onboarding/`). Register new compositions in `apps/remotion/src/remotion/Root.tsx`.
   - **Animated primitives/blocks**: add under `packages/remotion-lib/src/` (primitives, blocks, sections). Use `@repo/remotion-lib` from compositions when needed.

4. **Commands**  
   - Dev: `bun run dev --filter=remotion`  
   - Build: `bun run build` (root)  
   - Render: see [runbooks/remotion](remotion.md) for `remotion render`, stills, and codecs.

Do not duplicate full Remotion command reference here; link to the Remotion runbook.

---

## 04 – Conventions for video courses

- **Structure**: Prefer a clear flow: intro → concept explanation → code demo → recap.
- **Naming**: Use a consistent scheme, e.g. THP slug + module + lesson (e.g. `react-module-2-lesson-3-intro`). Keep composition IDs and file names aligned so they are discoverable.
- **Remotion primitives**: Reuse building blocks from `@repo/remotion-lib` and `@repo/ui/remotion`. For a detailed list of primitives and usage, see [runbooks/remotion](remotion.md); a dedicated reference doc (e.g. `video-ai/remotion-primitives.md`) may be added later.

---

## 05 – Feedback and iteration loop (future IA)

- **Where feedback lives**: User feedback is intended to be stored and keyed by video/course (e.g. composition ID or THP lesson identifier). Data model and storage are defined in the THP platform or a dedicated service; this repo does not own the persistence layer.
- **How devs use it**: Prioritize which videos to improve or regenerate based on feedback (and later, AI-suggested priorities). Implement changes in compositions or remotion-lib, then re-render and ship.
- **TODO**: When AI/skills and automation exist, add a how-to or runbook section and link it here.

---

## 06 – Quality, review, and deployment

- **Quality checklist for a new video**  
  - Code in compositions and remotion-lib is readable and follows repo conventions.  
  - Pacing and clarity are suitable for the target lesson.  
  - Audio (if any) is clear and consistent.  
  - Video is tested in the THP app (or equivalent consumer) before release.

- **PR review for new videos/components**  
  - **Code review**: Same as for other repo changes (lint, types, structure).  
  - **Pedagogical review**: Content and flow match the course intent; no regressions for learners.

- **Deployment**  
  - Rendered assets are published or deployed according to THP platform process (e.g. upload to CDN, trigger app update). Rendering and ops procedures will be documented in a future `video-ai-rendering.md` runbook when applicable.
