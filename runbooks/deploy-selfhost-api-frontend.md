---
title: "Deploy self-host (API + frontend + PostgreSQL)"
type: runbook
diataxis: how-to
status: draft
area: deployment
tags:
  - runbook
  - deployment
  - dokploy
  - coolify
  - vps
created: 2026-03-20
updated: 2026-03-20
related:
  - "[[runbooks/api]]"
  - "[[runbooks/frontend]]"
  - "[[runbooks/postgres-local]]"
---

# Runbook: self-host deploy

## Topology

- Service 1: `api` (Hono/Bun)
- Service 2: `frontend` (Vite static build)
- Service 3: `postgres`

## Required environment variables

- API: `DATABASE_URL`, `PORT`, `CORS_ORIGIN`, `NODE_ENV`
- Frontend: `VITE_API_BASE_URL`

## Healthchecks

- API: `GET /health`
- Frontend: HTTP 200 on `/`

## Rollback / annulation

- App rollback: redeploy previous image/tag for `api` and `frontend`.
- DB rollback:
  - Prefer additive migrations in forward-only mode.
  - If rollback required, run dedicated reverse migration and re-deploy previous app tag.

## Security checklist

- Keep credentials only in platform secrets.
- No `.env.local` in git.
- Restrict CORS to frontend public URL in production.
