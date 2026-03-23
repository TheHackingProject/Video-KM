---
title: "Deploy self-host (API + frontend + PostgreSQL + Remotion + Storybook)"
type: runbook
diataxis: how-to
status: published
area: deployment
tags:
  - runbook
  - deployment
  - dokploy
  - coolify
  - vps
  - docker
created: 2026-03-20
updated: 2026-03-23
related:
  - "[[runbooks/api]]"
  - "[[runbooks/frontend]]"
  - "[[runbooks/remotion]]"
  - "[[runbooks/storybook]]"
  - "[[runbooks/postgres-local]]"
  - "[[runbooks/monorepo]]"
---

# Runbook: self-host deploy

Procedure for deploying the Video-AI monorepo on a VPS using **Coolify** (or similar) with **one Docker image per app**, **PostgreSQL** on the same instance, and **repository root** as the Docker build context.

## Topology

| Resource | Role | Container port (typical) |
|----------|------|---------------------------|
| `postgres` | PostgreSQL for `DATABASE_URL` | 5432 (internal) |
| `api` | Hono + Bun (`apps/api`) | `8787` (or `PORT`) |
| `frontend` | Vite static + nginx | `80` |
| `storybook` | Static Storybook + nginx | `80` |
| `remotion` | Remotion Studio (dev server) | `3000` |

You can **disable or skip** a Coolify service (e.g. Storybook or Remotion) if you do not need it exposed; images are independent.

## Decisions (scope)

- **Several Coolify applications**, same Git repo/branch: each service has its own Dockerfile path; **build context = repository root** (`.`), so shared `packages/*` resolve correctly.
- **Alternative**: one Docker Compose stack — not documented here; this runbook assumes **separate resources** for simpler independent rollback.

## Build images (local / CI)

**Prerequisite**: Docker, repo cloned, `bun.lock` present.

**Context**: always the **monorepo root** (`Video-AI/`).

```bash
# API
docker build -f apps/api/Dockerfile -t video-ai-api:local .

# Frontend (set public API URL at build time)
docker build -f apps/frontend/Dockerfile \
  --build-arg VITE_API_BASE_URL=https://api.example.com \
  -t video-ai-frontend:local .

# Storybook (static site)
docker build -f apps/storybook/Dockerfile -t video-ai-storybook:local .

# Remotion Studio
docker build -f apps/remotion/Dockerfile -t video-ai-remotion:local .
```

**Smoke tests**:

```bash
docker run --rm -p 8787:8787 -e DATABASE_URL='postgres://u:p@host:5432/db' video-ai-api:local
curl -s http://127.0.0.1:8787/health

docker run --rm -p 8080:80 video-ai-frontend:local
curl -sI http://127.0.0.1:8080/ | head -1

docker run --rm -p 6007:80 video-ai-storybook:local
curl -sI http://127.0.0.1:6007/ | head -1

docker run --rm -p 3000:3000 video-ai-remotion:local
# Studio: http://127.0.0.1:3000 — first load can take >60s (webpack); healthcheck start_period is 120s.
```

### Monorepo notes

- **[`.dockerignore`](../../../.dockerignore)** (repo root) excludes `node_modules`, `KM/`, `packages/skills`, build artifacts, etc., to keep context small.
- **Storybook**: do **not** use `turbo run build --filter=storybook` for a static site — that runs **Next.js** `next build`. Use **`bun run build-storybook`** (Turbo task `build-storybook`) or the Storybook Dockerfile, which runs `turbo run build-storybook --filter=storybook`.
- **`packages/ui/tsconfig.json`**: inlined compiler options (aligned with `@repo/typescript-config` base + react-library) so **Vite/esbuild** inside Docker can bundle Storybook without failing on chained `extends` resolution.

## Coolify: several resources

For **each** application:

1. **Repository** + **branch** (same for all).
2. **Build type**: Dockerfile.
3. **Dockerfile path**: `apps/api/Dockerfile`, `apps/frontend/Dockerfile`, `apps/storybook/Dockerfile`, or `apps/remotion/Dockerfile`.
4. **Context**: **root** of the repository (not `apps/...`).
5. **Domains**: assign FQDNs per service (e.g. `app.`, `api.`, `storybook.`, `remotion.`).
6. **Internal network**: place **PostgreSQL** and **API** on the same Docker network; inject `DATABASE_URL` into the API from Coolify secrets.

### Ports to expose

- **API**: map host to container `8787` (or match `PORT`).
- **Frontend / Storybook**: map to container `80`.
- **Remotion**: map to container `3000`; ensure enough **CPU/RAM** (Studio uses webpack).

## Required environment variables

| Service | Variables |
|---------|-----------|
| **API** | `DATABASE_URL`, `PORT` (default `8787`), `CORS_ORIGIN` (public frontend origin), `NODE_ENV=production` |
| **Frontend** | Build arg `VITE_API_BASE_URL` (public API URL) — set in Coolify build arguments |
| **Storybook** | None required for static build |
| **Remotion** | Optional; `NODE_ENV=development` in image for Studio |

## Healthchecks

- **API**: `GET /health` → `200` JSON `{ "status": "ok" }`.
- **Frontend / Storybook (nginx)**: HTTP `200` on `/`.
- **Remotion**: HTTP `200` on `/` (slow cold start — image `HEALTHCHECK` uses `start-period=120s`).

## Database migrations

Schema and commands: [runbooks/postgres-local](postgres-local.md) (`db:migrate`, `db:push`, etc.).

Run migrations **once** after Postgres is up, e.g. one-off job or shell using the same `DATABASE_URL` as production (from a secure admin context — not in this runbook’s detail to avoid duplicating postgres-local).

## Rollback / annulation

- **Apps**: redeploy previous image/tag per Coolify resource (`api`, `frontend`, `storybook`, `remotion`).
- **DB**: prefer **forward-only** migrations; reverse migrations only with a dedicated script and coordinated app rollback.

## Troubleshooting

| Symptom | Check |
|---------|--------|
| Container **Restarting** | `docker logs <container>` (or Coolify logs); API often fails on bad `DATABASE_URL`. |
| Frontend calls wrong API | Rebuild frontend with correct **`VITE_API_BASE_URL`** build arg. |
| Storybook build fails in Docker | Confirm **`build-storybook`** is used, not `turbo run build --filter=storybook`. |
| Remotion OOM / timeout | Increase memory limit; Studio is heavier than static nginx services. |
| CORS errors | Set **`CORS_ORIGIN`** on API to the **public** frontend URL (scheme + host, no trailing path). |

## Security checklist

- Keep credentials only in platform secrets.
- No `.env` with secrets in git (see `.gitignore`).
- Restrict **CORS** to the real frontend origin in production.
- Expose **Storybook** and **Remotion** only on trusted networks or auth at the edge if they are internal tools.
