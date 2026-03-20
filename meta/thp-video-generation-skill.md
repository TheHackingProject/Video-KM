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
updated: 2026-03-23
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

- Copy: `mkdir -p .cursor/skills && cp -r packages/skills/thp-video-generation .cursor/skills/`
- Symlink: `mkdir -p .cursor/skills && ln -sf "$(pwd)/packages/skills/thp-video-generation" .cursor/skills/thp-video-generation`
- Optional: `ln -sf "$(pwd)/packages/skills/remotion-best-practices" .cursor/skills/remotion-best-practices` (requires submodule init above)

Confirm `.cursor/skills/thp-video-generation/SKILL.md` exists. Same approach as `thp-solarpunk-visual`. See `packages/skills/README.md`.
