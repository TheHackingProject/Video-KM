---
title: "DA THP — synthèse (charte courte)"
type: reference
diataxis: reference
status: published
area: video-ai
tags:
  - da
  - solarpunk
  - thp
  - video-ai
created: 2026-03-27
updated: 2026-03-31
related:
  - "[[reference/solarpunk-theme-decisions]]"
  - "[[Templates/thp-solarpunk-visual-checklist]]"
  - "[[Templates/pilot-outline]]"
  - "[[meta/thp-video-generation-skill]]"
  - "[[research/soul-recherche-visuelle]]"
  - "[[reference/video-lifecycle]]"
  - "[[02-video-ai-roadmap]]"
---

# DA THP — synthèse (charte courte)

**Purpose:** One-page **operational** anchor for THP / Video-AI visual direction. It **locks what already exists** (Solarpunk + motion kit) — **do not** invent a new aesthetic here. All detail lives in the linked canonical docs and code.

**Principle:** Centralize DA around **Solarpunk** + **`DemoShowcaseSolarpunk`**. **Storybook** is for **new static blocks** or **important variants** only — not a substitute for this page or for the Remotion reference clip.

---

## Four non-negotiable rules

1. **One scene = one dominant message.**
2. **Hero vs secondary** must be explicit in the pilot outline and text-role mapping — see [pilot-outline](../Templates/pilot-outline.md), [thp-video-generation SKILL](../../../packages/skills/thp-video-generation/SKILL.md), [library-matrix](../../../packages/skills/thp-video-generation/references/library-matrix.md).
3. **Rhythm: enter → hold → exit**; **text supports the visual**, not the other way around — see [remotion-best-practices rules](../../../packages/skills/remotion-best-practices/rules/) (animations, sequencing, text) via the official Remotion skill.
4. **No one-off styling off-brand**; color semantics = **Solarpunk tokens only** — [solarpunk.tokens.css](../../../packages/theme/solarpunk.tokens.css), [`solarTheme`](../../../packages/ui/src/lib/remotion/theme.ts), [solarpunk-theme-decisions](solarpunk-theme-decisions.md).

---

## Canonical links (do not duplicate)

| Resource | Role |
|----------|------|
| [solarpunk-theme-decisions](solarpunk-theme-decisions.md) | Normative theme decisions |
| [thp-solarpunk-visual-checklist](../Templates/thp-solarpunk-visual-checklist.md) | Pre-merge visual QA |
| [pilot-outline](../Templates/pilot-outline.md) | Script, hero, graph, Static UI gate, Ready for Remotion |
| [solarpunk.tokens.css](../../../packages/theme/solarpunk.tokens.css) | Web tokens |
| [theme.ts / `solarTheme`](../../../packages/ui/src/lib/remotion/theme.ts) | Remotion theme |
| [DemoShowcaseSolarpunk.tsx](../../../packages/ui/src/DemoShowcaseSolarpunk.tsx) | Motion kit UI (Solarpunk showcase) |
| [demo-showcase/config.ts](../../../packages/ui/src/lib/remotion/demo-showcase/config.ts) | Showcase timing + extended palette (aligned with decisions) |
| [demo-showcase README](../../../packages/ui/src/lib/remotion/demo-showcase/README.md) | Primitive matrix: showcase vs category demos (`TextDemo`, `TransitionsDemo`, …) |
| [DemoShowcaseSolarpunkDemo.tsx](../../../apps/remotion/src/remotion/compositions/demos/DemoShowcaseSolarpunkDemo.tsx) | Remotion composition wrapper |
| [thp-video-generation SKILL](../../../packages/skills/thp-video-generation/SKILL.md) | Pipeline, Soul, schematic idea |
| [remotion-best-practices SKILL](../../../packages/skills/remotion-best-practices/SKILL.md) | Official Remotion agent skill (`rules/*.md` for timing, text, sequencing) |
| [video-ai-development §03b](../runbooks/video-ai-development.md) | Operational procedure |
| [remotion runbook](../runbooks/remotion.md) | Commands, THP demo catalogue |
| [Soul — recherche visuelle](../research/soul-recherche-visuelle.md) | Creative / vulgarisation layer before locking scenes |

---

## Mermaid vs this charter vs reference clip

- **Mermaid** (e.g. [video-lifecycle — visual index](video-lifecycle.md#architecture-and-flows-visual-index)) = **process map**.
- **This page** = **DA rules** and where truth lives.
- **Reference clip** (below) = **proof** of render, timing, and kit — Mermaid is **not** a substitute for art direction.

---

## Remotion (good practice)

Use **`Sequence`** for time and layering. Keep motion **readable** and **consistent** with the shared kit. The reference clip should show **intended rhythm** and **final look**, not only a flat catalogue of effects — see [remotion.md — demo catalogue](../runbooks/remotion.md) if needed.

---

## Storybook policy

- **Yes** when adding a **new static** component or an **important variant** not already covered.
- **No** when **only reusing** existing `@repo/ui` pieces.
- **No** to “document DA” in Storybook **without** a new UI artifact.

**Storybook does not centralize DA** — this charter + the Remotion reference clip do.

---

## Reference clip (current)

**Default visual / motion reference:** `DemoShowcaseSolarpunk` ([`@repo/ui`](../../../packages/ui/src/DemoShowcaseSolarpunk.tsx)) composed as **`DemoShowcaseSolarpunkDemo`** — Remotion `compositionId` **`DemoShowcaseSolarpunk`** in [`Root.tsx`](../../../apps/remotion/src/remotion/Root.tsx), also registered in the frontend scene registry and seed where applicable.

**Two levels (do not merge into one fake “course”):**

1. **Motion / kit (primary)** — The showcase is the **unified** motion étalon: Solarpunk styling as **kit dressing**, not a fabricated THP lesson. Add **only** missing primitives that matter (e.g. diff, transitions); **do not** turn it into an exhaustive catalog — per-type depth lives in category demos ([remotion runbook — demo catalogue](../runbooks/remotion.md): `TextDemo`, `TransitionsDemo`, `DiagramsDemo`, …). **No artificial narrative** just to show every block.
2. **Pedagogical (B2, optional)** — A **separate** composition (e.g. Serie01 pilot) **only if** you must validate **hero/secondary**, **hierarchy**, and **full-lesson rhythm** in a real narrative — something the showcase is **not** meant to replace.

**Storybook** stays for **static** UI in isolation; it does **not** replace this charter or the Remotion clips above.

If the showcase plus existing pilots are **not enough** for that pedagogical bar, plan **B2** — out of scope for extending this one-page charter.

---

## Compact inventory

| Column | Content |
|--------|---------|
| **Existing (canonical)** | Solarpunk decisions, checklist, pilot template, tokens, `solarTheme`, DemoShowcase UI + config, DemoShowcaseSolarpunkDemo, thp-video-generation skill, `packages/ui` stories: `button`, `card`, `code`, `code-block-static`, `concept-slide`, `schematic-flow-chart-view`, `section-intro`, `title-card`, `thp-lucide` (`*.stories.tsx` under `packages/ui/src/`). |
| **Missing** | **TBD** — fill after each pilot review (gaps in static blocks or repeated motion patterns). |
| **Storybook vs Remotion demo** | New **static** primitive → Storybook first. **Timing / motion** pattern → `remotion-lib` or composition + [remotion demo catalogue](../runbooks/remotion.md); `@repo/remotion-lib` coverage remains **partial** (see [02-video-ai-roadmap](../02-video-ai-roadmap.md)). |

**Update rhythm:** After each significant pilot or kit change, adjust the **Missing** row or open a tracked task — do not let this table rot.

---

## Vigilance (keep this page thin)

**Length:** This file must stay a **short charter**. If it starts to read like a **catalog** or a **mini-runbook**, **stop adding here** — extend the **canonical** docs (decisions, runbooks, skills, templates) and **replace** growth on this page with **links** only.

**Execution risk:** The main failure mode is **not** missing documentation — it is **not applying** this frame on the next pilots. Hold the line with [pilot-outline](../Templates/pilot-outline.md), [thp-solarpunk-visual-checklist](../Templates/thp-solarpunk-visual-checklist.md), and review before merge.

---

## See also

- [02-video-ai-roadmap — DA → étalon → Trigger → Mastra](../02-video-ai-roadmap.md#da-etalon-orchestration-ia)
- [video-ai-preparation](../video-ai-preparation/video-ai-preparation.md) — formats and shortlist
