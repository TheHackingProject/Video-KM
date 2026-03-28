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
updated: 2026-03-28
related:
  - "[[00-architecture]]"
  - "[[runbooks/monorepo]]"
  - "[[runbooks/postgres-local]]"
  - "[[runbooks/deploy-selfhost-api-frontend]]"
  - "[[runbooks/video-ai-rendering]]"
  - "[[reference/video-ai-orchestrator-decision]]"
---

# Runbook: API (`apps/api`)

Generic backend API for Video-AI platform. It is **not** the Trigger.dev platform: task definitions live in **`apps/trigger`** (workspace / package tasks); the platform runs **only on infrastructure we operate** (Compose upstream), not Trigger.dev Cloud — see [`infra/trigger-hosting`](../../../infra/trigger-hosting/README.md).

## Endpoints (v1)

- `GET /health`
- `GET /videos`
- `GET /videos/:slug`
- `POST /jobs/render-pipeline` — enqueue le pipeline Trigger.dev **prepare → render (stub) → notify** (`202` + `{ message, id }`). Retourne **`503`** si **`TRIGGER_SECRET_KEY`** ou **`TRIGGER_API_URL`** est absent (politique **souveraine** : pas d’appel implicite au cloud Trigger). Corps JSON : `compositionId` (optionnel, défaut `MyComp`), `correlationId` (optionnel) — schéma `@repo/contracts` `RenderPipelinePayloadSchema`.

Implémentation : **`tasks.trigger()`** avec **import de types uniquement** depuis le workspace `trigger/render-pipeline` — **aucun import runtime** du code des tasks dans le bundle API (voir [Triggering](https://trigger.dev/docs/triggering)).

## Local run

From repo root:

```bash
DATABASE_URL="postgres://video_ai:video_ai@localhost:5432/video_ai_dev" \
PORT=8787 \
CORS_ORIGIN="http://localhost:5173" \
TRIGGER_API_URL="http://localhost:8030" \
TRIGGER_SECRET_KEY="tr_dev_..." \
bun run dev --filter=api
```

## Validation and quality

```bash
bun run lint --filter=api
bun run check-types --filter=api
bun run build --filter=api
cd apps/api && bun test
```

## Error contract

- `404` for unknown routes and unknown video slug.
- `500` for unexpected runtime errors.

## Environment variables

- `DATABASE_URL` (required)
- `PORT` (default: `8787`)
- `CORS_ORIGIN` (default: `http://localhost:5173`)

### Trigger.dev (pré-v2 / POC orchestration)

**Politique projet** : **souveraineté** — orchestration Trigger **uniquement** sur **notre** instance (Compose upstream auto-hébergée). **Pas d’usage de Trigger.dev Cloud** pour Video-AI. Limitations **self-host** vs SaaS : voir doc Trigger officielle.

- **`TRIGGER_SECRET_KEY`** — **requis** pour `POST /jobs/render-pipeline` (clé API projet depuis le dashboard de **notre** instance).
- **`TRIGGER_API_URL`** — **requis** pour `POST /jobs/render-pipeline` : URL de base du webapp Trigger **self-host** (ex. `https://trigger.example.com`, ou `http://localhost:8030` en local). Garantit que le SDK ne cible **jamais** le cloud par défaut.
- **`TRIGGER_PROJECT_REF`** — utilisé par **`apps/trigger/trigger.config.ts`** pour `trigger dev` / `deploy` (pas embarqué comme fichier dans l’image API).

**CI** (déploiement automatisé des tasks) : `TRIGGER_ACCESS_TOKEN` + **`TRIGGER_API_URL`** vers **notre** instance uniquement — [GitHub Actions Trigger](https://trigger.dev/docs/github-actions).

**Dev / deploy tasks** : avec **Node** pour `npx trigger.dev@latest` :

- **Monorepo** : `bun run dev` (racine) lance aussi le **`dev`** du workspace **`trigger`** via Turbo (en parallèle des autres apps).
- **Trigger seul** : `bun run trigger:dev` à la racine, ou `cd apps/trigger && bun run dev`.

```bash
cd apps/trigger
npx trigger.dev@latest login -a http://localhost:8030
bun run dev
# ou: bun run trigger:deploy
```

Voir [video-ai-rendering](video-ai-rendering.md), [trigger-dev-coolify-spike](trigger-dev-coolify-spike.md), [infra/trigger-hosting](../../../infra/trigger-hosting/README.md), [video-ai-orchestrator-decision](../reference/video-ai-orchestrator-decision.md).

## Production (Docker)

- **Dockerfile**: [`apps/api/Dockerfile`](../../../apps/api/Dockerfile) (build from repo root).
- **Image**: `oven/bun` multi-stage; runs bundled `dist/index.js` with Bun.
- **Deploy procedure**: [runbooks/deploy-selfhost-api-frontend](deploy-selfhost-api-frontend.md).
- Pour `POST /jobs/render-pipeline` : injecter **`TRIGGER_API_URL`** (URL publique **self-host**) et **`TRIGGER_SECRET_KEY`** sur le service API Coolify — **obligatoires** ; pas de déploiement API « jobs » sans les deux.
