---
title: "API Runbook (apps/api)"
type: runbook
diataxis: how-to
status: published
area: backend
tags:
  - runbook
  - api
  - hono
  - bun
created: 2026-03-20
updated: 2026-03-20
related:
  - "[[00-architecture]]"
  - "[[runbooks/monorepo]]"
  - "[[runbooks/postgres-local]]"
---

# Runbook: API (`apps/api`)

Generic backend API for Video-AI platform.

## Endpoints (v1)

- `GET /health`
- `GET /videos`
- `GET /videos/:slug`

## Local run

From repo root:

```bash
DATABASE_URL="postgres://video_ai:video_ai@localhost:5432/video_ai_dev" \
PORT=8787 \
CORS_ORIGIN="http://localhost:5173" \
bun run dev --filter=api
```

## Validation and quality

```bash
bun run lint --filter=api
bun run check-types --filter=api
bun run build --filter=api
bun run test --filter=api
```

## Error contract

- `404` for unknown routes and unknown video slug.
- `500` for unexpected runtime errors.

## Environment variables

- `DATABASE_URL` (required)
- `PORT` (default: `8787`)
- `CORS_ORIGIN` (default: `http://localhost:5173`)
