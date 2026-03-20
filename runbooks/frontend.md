---
title: "Frontend Runbook (apps/frontend)"
type: runbook
diataxis: how-to
status: published
area: frontend
tags:
  - runbook
  - frontend
  - vite
  - react
created: 2026-03-20
updated: 2026-03-20
related:
  - "[[00-architecture]]"
  - "[[runbooks/monorepo]]"
  - "[[runbooks/api]]"
---

# Runbook: Frontend (`apps/frontend`)

Generic frontend app for the public video library.

## Features (v1)

- Video catalogue list
- Video detail page
- Embedded player + link to related KM docs

## Local run

From repo root:

```bash
VITE_API_BASE_URL="http://localhost:8787" bun run dev --filter=frontend
```

## Validation

```bash
bun run lint --filter=frontend
bun run check-types --filter=frontend
bun run build --filter=frontend
bun run test --filter=frontend
```

## Environment variables

- `VITE_API_BASE_URL` (default: `http://localhost:8787`)
