---
title: "Video-AI Preparation"
type: documentation
diataxis: reference
status: published
area: video-ai
tags:
  - video-ai
  - preparation
  - write-before-code
  - formats
  - components
created: 2026-03-12
updated: 2026-03-20
related:
  - "[[reference/video-lifecycle]]"
  - "[[explanation/video-ai-vision]]"
  - "[[runbooks/video-ai-development]]"
  - "[[00-architecture]]"
  - "[[Templates/pilot-outline]]"
  - "[[Templates/thp-solarpunk-visual-checklist]]"
  - "[[reference/solarpunk-theme-decisions]]"
  - "[[meta/thp-video-generation-skill]]"
  - "[[meta/thp-solarpunk-visual-skill]]"
---

# Video-AI Preparation

Write before code: this doc defines formats, component shortlist, and pilot outline before any Remotion components or compositions are added. It is the **single reference** for formats and shortlist — runbooks refer here and do not duplicate this content. The **reading order is contractual**: [Formats](#video-formats) → [Component shortlist](#component-shortlist) → [Pilot outline](#pilot-outline). Full workflow: [video-lifecycle](reference/video-lifecycle.md).

---

## Formats {#video-formats}

Definition of the 1–2 pedagogical video formats used for THP course videos. Each format has a pedagogical goal, target duration, structure, and visual needs. Use these to derive the component shortlist before writing Remotion code.

### Format 1 – Concept intro (short)

- **Pedagogical goal**: Introduce a single concept in under 1 minute so learners are not overwhelmed; one idea per clip.
- **Target duration**: 30–60 s.
- **Structure**: intro (hook) → concept (one idea) → optional micro-recap.
- **Visual needs**: title card, simple text or bullet list, minimal animation, optional callout.
- **Tone and level**: Target beginners in THP courses; avoid jargon unless it is the concept being introduced.
- **Pairing**: Often paired with a "Code demo guided" video on the same concept.
- **Example**: e.g. "What is HTTP?" intro.

### Format 2 – Code demo guided

- **Pedagogical goal**: Walk through code step-by-step so learners can follow and replay at their own pace.
- **Target duration**: 2–5 min (to be set per lesson).
- **Structure**: intro → concept reminder → code walkthrough (explanation + code + line-by-line highlight) → recap.
- **Visual needs**: title/section titles, code block with highlight, callouts, progress or step indicator, transitions between steps.
- **Scope**: Focus on one main outcome (e.g. build X, understand Y). Do not introduce unrelated new concepts in the same clip.
- **Context**: Aim for the video to be understandable with light course context, but not to replace the written lesson entirely.
- **Example**: e.g. "Build a simple HTML page" walkthrough.

### Out of scope (formats)

- Live-coding screen recordings without scripted structure (these formats focus on scripted, composited videos built with Remotion).
- Non-pedagogical marketing or onboarding videos (handled by other formats if needed later).

Next: derive the component shortlist below, then implement P0 components in `packages/remotion-lib` before building compositions in `apps/remotion`.

---

## Component shortlist {#component-shortlist}

Derived from [Formats](#video-formats). Implement in `packages/remotion-lib` when ready. **Build P0 first for the first pilot** (3–4 components); do not tackle many in parallel.

### Legos (primitives)

Stateless or minimally stateful building blocks; do not encode lesson-specific logic.

| Component | Priority | Notes |
|-----------|----------|--------|
| TitleCard | P0 | Title + optional subtitle; static or simple fade-in/out, no complex animation. Used in both formats. |
| SectionIntro | P0 | Short intro line or hook for a section; inline with content, short text, maybe with subtle enter animation. |
| CodeBlockWithHighlight | P0 | Code + line-by-line highlight; supports stepping through lines/regions over time, driven by props (steps) rather than hard-coded timings. Core for Format 2. |
| BulletList | P1 | Simple bullet list for concept slides. |
| Callout | P1 | Highlight box or callout for key points. |
| ProgressBar | P1 | Step or progress indicator for code walkthrough. |

### Pedagogical patterns

Small orchestrators that combine legos into reusable teaching patterns; still generic (no THP-specific copy or layout).

| Component | Priority | Notes |
|-----------|----------|--------|
| ConceptSlide | P0 | Orchestrates title + text/bullets + optional callout; for Format 1. |
| CodeAlongStep | P0 | One step in a code demo: composes one CodeBlockWithHighlight + a short text area; single responsibility = one step of the demo. For Format 2. |
| QuizSlide | P1 | Question + options (future); not required for first pilot. |

### Implementation order

1. **P0 legos**: TitleCard, SectionIntro, CodeBlockWithHighlight. Implement with minimal, opinionated defaults (font sizes, spacing) to be good enough for the pilot.
2. **P0 patterns**: ConceptSlide, CodeAlongStep (using the P0 legos). Keep props small (title, body, code, steps) and avoid config explosion in v1.
3. **Pilots**: Use them in 1–2 pilot compositions in `apps/remotion`. Only after at least one pilot is shipped, revisit whether P1 items are still relevant.

### Out of scope for this shortlist

- THP-specific branding wrappers (handled in UI layer if needed).
- Low-level layout/grid components (reused from existing UI library where possible).

---

## Pilot outline {#pilot-outline}

Use the **pilot outline template** to define script and scene breakdown before implementing in Remotion. Copy the template per pilot, fill it in with a real THP course extract, then implement once the checklist is done.

- **Template (canonical structure)**: [Templates/pilot-outline.md](../Templates/pilot-outline.md) — copy per pilot (e.g. into this folder or a working folder). The template’s `updated` frontmatter and body are the **source of truth** for required sections and checklists; pilot copies must stay aligned when the template evolves.
- **THP visual identity**: all pilots follow **Solarpunk dark** ([solarpunk-theme-decisions](../reference/solarpunk-theme-decisions.md)); before delivery, use [thp-solarpunk-visual-checklist](../Templates/thp-solarpunk-visual-checklist.md).

### What each pilot copy must contain (matches the template) {#pilot-copy-structure}

When you copy [pilot-outline](../Templates/pilot-outline.md), the filled file should cover the same blocks (expand or trim text, but do not drop structural sections without a conscious decision):

| Block | Purpose |
|-------|---------|
| **Pilot metadata** | Title, format, target duration, **source**, **paired video** (cross-links between Concept intro and Code demo when applicable). |
| **Script** | Full or linked script; **structure hint** aligned with Format 1 vs Format 2 ([Formats](#video-formats)). |
| **Scene breakdown** | One row per scene: purpose, estimated duration, component(s) from [Component shortlist](#component-shortlist); total duration check vs target. |
| **Components needed** | Table mapping shortlist components to scene numbers; note gaps if something is missing from the shortlist. |
| **Ready for Remotion when** | Checklist: script + breakdown review, shortlist alignment, format/duration consistency, Solarpunk checklist, **agent setup** (`bootstrap:agents`, three skills + canonical prompt — see [video-ai-development §08](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo)). |

### Agents (Cursor / multi-agent) {#pilot-outline-agents}

For any work on **script**, **scene breakdown**, **timings**, or **Remotion code**, the template expects:

1. **One-time per clone**: `bun run bootstrap:agents` from repo root (submodules + `.cursor/skills/`). See [video-ai-development §08](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo) and [`.cursor/environment.json`](../../../.cursor/environment.json) for Background Agents.
2. **Three Agent Skills** loaded explicitly: [THP video generation](../meta/thp-video-generation-skill.md), [THP Solarpunk visual](../meta/thp-solarpunk-visual-skill.md), **remotion-best-practices** (rules under `packages/skills/remotion-best-practices/rules/` as needed).
3. **Canonical session prefix** (paste at the top of the chat): same wording as in [Templates/pilot-outline.md](../Templates/pilot-outline.md) (*Avec un agent Cursor*) and [video-ai-development §08 — Triggers agent](../runbooks/video-ai-development.md#triggers-agent-phrases--rules-remotion).

- **Usage**: Fill in metadata, script, scene breakdown, and components; only then create or edit compositions in `apps/remotion`.

### Template sync before script edits

In a **feedback loop** (V0.x → V0.y, retours pédago, etc.), **do not** change the pilot script, scene breakdown, or Remotion implementation until you have confirmed the outline file is still aligned with the canonical template.

**Before each iteration that touches the script or scenes:**

1. Open [Templates/pilot-outline.md](../Templates/pilot-outline.md) and note the `updated` field in the frontmatter (and any new sections or checklist items).
2. Compare with your pilot file under `KM/Docs/video-ai-preparation/` (e.g. `pilot-01-prerequis-outline.md`):
   - `git log` / `git diff` on `KM/Docs/Templates/pilot-outline.md` since the last time you synced this pilot, **or** diff the template against your copy’s structure (metadata table, “Ready for Remotion when”, agent bullet list).
3. **If the template changed** (new rows, new checklist lines, new agent steps, renamed headings): **update the pilot copy first** — merge in the missing structure, tick lists, or wording — then revise script / tables / Remotion in **this** iteration from the updated outline.
4. **If the template is unchanged**: proceed with script or composition edits as planned.

This avoids drift: the repo template gains improvements over time; pilot files that skip this step silently miss new mandatory checks.

See [video-lifecycle](reference/video-lifecycle.md) for where the pilot outline fits in the full sequence. The same **sync-before-script** rule is repeated in [video-ai-development runbook §03b](../runbooks/video-ai-development.md) (*Itération — synchroniser le template*).
