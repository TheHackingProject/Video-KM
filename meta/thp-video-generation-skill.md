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
- **Diagram pipeline (EN)** : [`packages/skills/thp-video-generation/references/diagram-asset-pipeline.md`](../../../packages/skills/thp-video-generation/references/diagram-asset-pipeline.md) — checklist versionnée par vidéo : [`KM/Docs/video-ai-preparation/diagrams/`](../video-ai-preparation/diagrams/README.md)
- **Skills index** : [`packages/skills/README.md`](../../../packages/skills/README.md)

This KM page is a **shortcut** for Obsidian and cross-links from other docs.

**Cursor** (each machine / after clone), from the **Video-AI repo root**:

- Copy: `mkdir -p .cursor/skills && cp -r packages/skills/thp-video-generation .cursor/skills/`
- Symlink: `mkdir -p .cursor/skills && ln -sf "$(pwd)/packages/skills/thp-video-generation" .cursor/skills/thp-video-generation`

Confirm `.cursor/skills/thp-video-generation/SKILL.md` exists. Same approach as `thp-solarpunk-visual`. See `packages/skills/README.md`.
