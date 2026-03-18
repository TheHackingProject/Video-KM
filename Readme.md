---
title: "Knowledge Base (KM/Docs) Overview"
type: documentation
diataxis: reference
status: published
area: documentation
tags:
  - documentation
  - knowledge-base
  - cursor
created: 2026-03-10
updated: 2026-03-11
related:
  - "[[00-architecture]]"
  - "[[01-index]]"
  - "[[runbooks/monorepo]]"
  - "[[runbooks/storybook]]"
---

# KM/Docs – Knowledge Base

Project documentation and runbooks for Video-AI.

## Quick links

- [**00-architecture**](00-architecture.md) – Monorepo structure and repository links
- [**01-index**](01-index.md) – Full documentation index
- [**Video-AI**](reference/video-lifecycle.md) – Canonical reference (role, lifecycle) · [vision](explanation/video-ai-vision.md) · [development runbook](runbooks/video-ai-development.md)
- [**video-lifecycle**](reference/video-lifecycle.md) – Full lifecycle (idea → preparation → components → scene → review → render → feedback)
- [**video-ai-preparation**](video-ai-preparation/video-ai-preparation.md) – Write before code (formats, shortlist, pilot outline)
- [**Runbooks**](runbooks/) – Procedures (monorepo, remotion, video-ai-development, [GitHub MCP projects](runbooks/github-mcp-projects.md), etc.)

Video-AI workflow (condensate): prepare (formats, script) in video-ai-preparation → build or reuse components (remotion-lib) → compose scene (apps/remotion) → review → render. Full reference: [video-lifecycle](reference/video-lifecycle.md).

## Doc classification (Diataxis)

Docs are classified by type via the `diataxis` frontmatter field; each page has a clear role. See [diataxis.fr](https://diataxis.fr/).

| Page type    | Diataxis    | Use when |
|--------------|-------------|----------|
| Reference    | reference   | What / where / order (facts, structure) |
| Explanation  | explanation| Why / trajectory / context |
| How-to       | how-to     | Procedure, steps (runbooks) |
| Tutorial     | tutorial   | Learning path (if used) |
