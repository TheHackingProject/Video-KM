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
updated: 2026-03-27
related:
  - "[[meta/thp-video-generation-skill]]"
  - "[[00-architecture]]"
  - "[[reference/video-ai-orchestrator-decision]]"
  - "[[reference/video-ai-upper-layers-mastra-openclaw]]"
  - "[[02-video-ai-roadmap]]"
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

**Quick path — plan & statut** : après cet index, ouvre directement [**02-video-ai-roadmap**](02-video-ai-roadmap.md) (roadmap et état des lieux v1 → v2).

**Linking**: Point to sections (anchors) of living docs; when changing file structure, prefer adding anchors and updating links over duplicating content. Example: do not create a separate quick-reference file (e.g. `video-ai-lifecycle-quick.md`); add an anchor or a short paragraph in the canonical reference page and link to it.

## Architecture & structure

- [**00-architecture**](./00-architecture.md) – Directory tree, locations, repository links

## Video-AI project

- [**02-video-ai-roadmap**](./02-video-ai-roadmap.md) – **Roadmap & état des lieux** (v1 → v1.1 → pré-v2 → v2 ; livré vs à faire) — **à la racine KM/Docs**, suite de `01-index`
- [**video-ai-orchestrator-decision**](./reference/video-ai-orchestrator-decision.md) – **Décision orchestrateur** : Trigger.dev v4 (vs Inngest), spike Coolify
- [**video-ai-upper-layers-mastra-openclaw**](./reference/video-ai-upper-layers-mastra-openclaw.md) – **Mastra + OpenClaw** au-dessus de Trigger (v2, HITL, poste auteur)
- [**video-lifecycle**](./reference/video-lifecycle.md) – Canonical reference: role, lifecycle, where it lives (idea → preparation → components → scene → review → render → THP → feedback → iteration)
- [**video-ai-vision**](./explanation/video-ai-vision.md) – Long-term vision, v1/v2/v3, pedagogy (explanation)
- [**video-ai-preparation**](./video-ai-preparation/video-ai-preparation.md) – Preparation (formats, shortlist, pilot outline) – write before code

## Research

Exploratory notes, evaluations, and POCs. **Not canonical**; see [reference/tools](./reference/tools/README.md) for stable tool reference. New tools and patterns start in research/; promote to reference/tools/ only once adopted or officially recommended.

- [**research/**](./research/README.md) – Overview and rules
- [**Soul — visuel & vulgarisation**](./research/soul-recherche-visuelle.md) · [**Git/GitHub (visuel)**](./research/git-github-vulgarisation-visuelle.md)
- [**openclaw**](./research/openclaw.md) · [**mastra**](./research/mastra.md) · [**trigger-dev**](./research/trigger-dev.md) · [**inngest**](./research/inngest.md)

**Reference (tools):** [reference/tools/](./reference/tools/README.md) – Stable tool reference (one file per adopted tool); promoted from research when decided.

## Runbooks (procedures)

- [**monorepo**](./runbooks/monorepo.md) – Turborepo, structure, scripts, usage
- [**dependencies-submodules**](./runbooks/dependencies-submodules.md) – Submodules, integrating GitHub repositories
- [**bun-biome**](./runbooks/bun-biome.md) – Bun, Biome (lint/format), CLI gotcha
- [**storybook**](./runbooks/storybook.md) – Storybook, component library documentation
- [**remotion**](./runbooks/remotion.md) – Remotion Studio, video generation, compositions
- [**video-ai-development**](./runbooks/video-ai-development.md) – Video-AI Development Runbook (purpose, workflow, conventions, quality)
- [**api**](./runbooks/api.md) – Generic backend API (`apps/api`) with Hono/Bun
- [**frontend**](./runbooks/frontend.md) – Generic frontend (`apps/frontend`) with Vite/React
- [**postgres-local**](./runbooks/postgres-local.md) – Local PostgreSQL + Drizzle migrations/seed/reset
- [**deploy-selfhost-api-frontend**](./runbooks/deploy-selfhost-api-frontend.md) – Coolify/VPS: Docker images per app (api, frontend, storybook, remotion), Postgres, build context, troubleshooting

## Agent skills (versioned in repo)

- **THP Video generation** — source: `packages/skills/thp-video-generation/`; KM pointer: [meta/thp-video-generation-skill](meta/thp-video-generation-skill.md); **triggers + rules Remotion** : [video-ai-development §08](runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo).
- **THP Solarpunk visual** — [meta/thp-solarpunk-visual-skill](meta/thp-solarpunk-visual-skill.md) — utiliser **avec** remotion-best-practices sur les passes Remotion.
- **Remotion (remotion-best-practices)** — submodule `packages/skills/Remotion`, règles détaillées dans `packages/skills/remotion-best-practices/rules/` ; même contenu que le dépôt officiel Remotion Agent Skills.

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
