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
updated: 2026-03-27
related:
  - "[[00-architecture]]"
  - "[[runbooks/monorepo]]"
  - "[[runbooks/postgres-local]]"
  - "[[runbooks/deploy-selfhost-api-frontend]]"
  - "[[runbooks/video-ai-rendering]]"
  - "[[reference/video-ai-orchestrator-decision]]"
---

# Runbook: API (`apps/api`)

Generic backend API for Video-AI platform.

## Endpoints (v1)

- `GET /health`
- `GET /videos`
- `GET /videos/:slug`
- `POST /jobs/render-pipeline` — enqueue le pipeline Trigger.dev **prepare → render (stub) → notify** (`202` + `{ message, id }`). Retourne `503` si `TRIGGER_SECRET_KEY` est absent. Corps JSON : `compositionId` (optionnel, défaut `MyComp`), `correlationId` (optionnel) — schéma `@repo/contracts` `RenderPipelinePayloadSchema`.

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

### Trigger.dev (pré-v2 / POC orchestration)

- `TRIGGER_SECRET_KEY` — requis pour `POST /jobs/render-pipeline` (secret projet, dashboard Trigger.dev).
- `TRIGGER_PROJECT_REF` — référence projet (ex. `proj_xxx`) ; utilisée par `trigger.config.ts`.
- `TRIGGER_API_URL` — optionnel (override endpoint API cloud / self-host).

**Dev tasks** : depuis `apps/api`, avec Node disponible pour `npx` :

```bash
cd apps/api
bun run trigger:dev
```

Voir [video-ai-rendering](video-ai-rendering.md), [trigger-dev-coolify-spike](trigger-dev-coolify-spike.md), [video-ai-orchestrator-decision](../reference/video-ai-orchestrator-decision.md).

## Production (Docker)

- **Dockerfile**: [`apps/api/Dockerfile`](../../../apps/api/Dockerfile) (build from repo root).
- **Image**: `oven/bun` multi-stage; runs bundled `dist/index.js` with Bun.
- **Deploy procedure**: [runbooks/deploy-selfhost-api-frontend](deploy-selfhost-api-frontend.md).
