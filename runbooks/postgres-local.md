---
title: "PostgreSQL local runbook"
type: runbook
diataxis: how-to
status: published
area: database
tags:
  - runbook
  - postgres
  - drizzle
  - database
created: 2026-03-20
updated: 2026-03-20
related:
  - "[[runbooks/api]]"
  - "[[runbooks/monorepo]]"
---

# Runbook: PostgreSQL local

This project uses PostgreSQL with Drizzle in `packages/db`.

## Local setup (example)

```bash
sudo -u postgres createuser --login video_ai || true
sudo -u postgres psql -d postgres -c "ALTER ROLE video_ai WITH PASSWORD 'video_ai';"
sudo -u postgres createdb --owner=video_ai video_ai_dev || true
```

## Connection string

Use:

```bash
DATABASE_URL=postgres://video_ai:video_ai@localhost:5432/video_ai_dev
```

## DB commands

```bash
# generate migration SQL from schema
bun run db:generate --filter=@repo/db

# apply migrations
DATABASE_URL="postgres://video_ai:video_ai@localhost:5432/video_ai_dev" bun run db:migrate --filter=@repo/db

# seed initial data (Showcase + Pilot 01 + Pilot 02)
DATABASE_URL="postgres://video_ai:video_ai@localhost:5432/video_ai_dev" bun run db:seed --filter=@repo/db

# reset local DB content
DATABASE_URL="postgres://video_ai:video_ai@localhost:5432/video_ai_dev" bun run db:reset --filter=@repo/db
```
