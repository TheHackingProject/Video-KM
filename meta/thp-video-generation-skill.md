---
title: "THP Video generation skill — versioned pointer"
type: documentation
area: meta
status: published
tags:
  - cursor
  - skill
  - video-ai
  - thp
created: 2026-03-21
updated: 2026-03-20
related:
  - "[[runbooks/video-ai-development]]"
  - "[[reference/solarpunk-theme-decisions]]"
---

# THP Video generation skill

**Canonical skill content** lives in the monorepo:

- **Entry** : [`packages/skills/thp-video-generation/SKILL.md`](../../../packages/skills/thp-video-generation/SKILL.md)
- **Block / demo matrix** : [`packages/skills/thp-video-generation/references/library-matrix.md`](../../../packages/skills/thp-video-generation/references/library-matrix.md)
- **Diagram pipeline (EN)** : [`packages/skills/thp-video-generation/references/diagram-asset-pipeline.md`](../../../packages/skills/thp-video-generation/references/diagram-asset-pipeline.md) — option Mermaid→SVG ; par défaut **schéma React** : [`SchematicFlowChartView`](../../../packages/ui/src/lib/diagrams/SchematicFlowChartView.tsx) (Storybook) + `FlowChart` dans [`DiagramsDemo`](../../../apps/remotion/src/remotion/compositions/demos/DiagramsDemo.tsx).
- **Skills index** : [`packages/skills/README.md`](../../../packages/skills/README.md)

This KM page is a **shortcut** for Obsidian and cross-links from other docs.

**Remotion upstream skill** : initialize submodule **`packages/skills/Remotion`** (`git submodule update --init packages/skills/Remotion` or full `--recursive`). Symlinks: `packages/skills/remotion-best-practices` → `Remotion/skills/remotion` — see [`packages/skills/README.md`](../../../packages/skills/README.md).

**Cursor** (each machine / after clone), from the **Video-AI repo root**:

- **Recommended:** `bun run bootstrap:agents` (submodule Remotion + symlinks under `.cursor/skills/` for `thp-video-generation` and `remotion-best-practices`). See [`.cursor/environment.json`](../../../.cursor/environment.json) for Background Agents.
- Manual: `mkdir -p .cursor/skills && ln -sf "$(pwd)/packages/skills/thp-video-generation" .cursor/skills/thp-video-generation` — puis optionnel `remotion-best-practices` comme dans [`packages/skills/README.md`](../../../packages/skills/README.md).

Confirm `.cursor/skills/thp-video-generation/SKILL.md` exists. `thp-solarpunk-visual` : symlink séparé — voir [meta/thp-solarpunk-visual-skill](thp-solarpunk-visual-skill.md).

**Trigger workflow (fort)** : pour toute session qui modifie une composition ou des timings, coller en tête du chat la **phrase canonique** et la **carte rules → tâche** dans [runbooks/video-ai-development §08 — Triggers agent](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo). Toujours associer ce skill à **thp-solarpunk-visual** et **remotion-best-practices** sur du code Remotion.
