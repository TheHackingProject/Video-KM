---
title: "Documentation Index"
type: documentation
status: published
area: documentation
tags:
  - documentation
  - index
  - runbooks
  - architecture
created: 2026-03-10
updated: 2026-03-11
related:
  - "[[00-architecture]]"
  - "[[reference/video-lifecycle]]"
  - "[[reference/tools/README]]"
  - "[[explanation/video-ai-vision]]"
  - "[[research/README]]"
  - "[[runbooks/video-ai-development]]"
  - "[[runbooks/monorepo]]"
  - "[[runbooks/dependencies-submodules]]"
  - "[[runbooks/bun-biome]]"
  - "[[runbooks/storybook]]"
  - "[[runbooks/remotion]]"
---

# 01 – Documentation Index

Index of project documentation resources.

**Linking**: Point to sections (anchors) of living docs; when changing file structure, prefer adding anchors and updating links over duplicating content. Example: do not create a separate quick-reference file (e.g. `video-ai-lifecycle-quick.md`); add an anchor or a short paragraph in the canonical reference page and link to it.

## Architecture & structure

- [**00-architecture**](./00-architecture.md) – Directory tree, locations, repository links

## Video-AI project

- [**video-lifecycle**](./reference/video-lifecycle.md) – Canonical reference: role, lifecycle, where it lives (idea → preparation → components → scene → review → render → THP → feedback → iteration)
- [**video-ai-vision**](./explanation/video-ai-vision.md) – Long-term vision, v1/v2/v3, pedagogy (explanation)
- [**video-ai-preparation**](./video-ai-preparation/video-ai-preparation.md) – Preparation (formats, shortlist, pilot outline) – write before code

## Research

Exploratory notes, evaluations, and POCs. **Not canonical**; see [reference/tools](./reference/tools/README.md) for stable tool reference. New tools and patterns start in research/; promote to reference/tools/ only once adopted or officially recommended.

- [**research/**](./research/README.md) – Overview and rules
- [**openclaw**](./research/openclaw.md) · [**mastra**](./research/mastra.md) · [**trigger-dev**](./research/trigger-dev.md) · [**inngest**](./research/inngest.md)

**Reference (tools):** [reference/tools/](./reference/tools/README.md) – Stable tool reference (one file per adopted tool); promoted from research when decided.

## Runbooks (procedures)

- [**monorepo**](./runbooks/monorepo.md) – Turborepo, structure, scripts, usage
- [**dependencies-submodules**](./runbooks/dependencies-submodules.md) – Submodules, integrating GitHub repositories
- [**bun-biome**](./runbooks/bun-biome.md) – Bun, Biome (lint/format), CLI gotcha
- [**storybook**](./runbooks/storybook.md) – Storybook, component library documentation
- [**remotion**](./runbooks/remotion.md) – Remotion Studio, video generation, compositions
- [**video-ai-development**](./runbooks/video-ai-development.md) – Video-AI Development Runbook (purpose, workflow, conventions, quality)

## Templates

Obsidian templates for new documentation files (Settings → Templates → folder: `Templates`):

- [**frontmatter-doc**](./Templates/frontmatter-doc.md) – Standard documentation
- [**frontmatter-runbook**](./Templates/frontmatter-runbook.md) – Runbook/procedure
- [**frontmatter-adr**](./Templates/frontmatter-adr.md) – Architecture Decision Record
- [**frontmatter-research-topic**](./Templates/frontmatter-research-topic.md) – Research topic (one file per tool/subject)
- [**pilot-outline**](./Templates/pilot-outline.md) – Video-AI pilot video outline (script, scenes, components)

## External links

- [Turborepo – docs](https://turborepo.dev/docs/getting-started)
- [Biome – docs](https://biomejs.dev/guides/getting-started/)
- [Remotion – docs](https://www.remotion.dev/docs)
- [Storybook – docs](https://storybook.js.org/docs)
