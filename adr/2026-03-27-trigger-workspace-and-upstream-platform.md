---
title: "ADR-2026-03-27: Trigger.dev — workspace tasks vs plateforme upstream"
type: adr
diataxis: explanation
status: accepted
area: architecture
tags:
  - adr
  - architecture
  - trigger-dev
  - video-ai
created: 2026-03-27
updated: 2026-03-28
related:
  - "[[reference/video-ai-orchestrator-decision]]"
  - "[[runbooks/api]]"
  - "[[runbooks/trigger-dev-coolify-spike]]"
---

# ADR: Trigger.dev — workspace tasks vs plateforme upstream

## Status

Accepted

## Context

Trigger.dev v4 peut être déployé en **self-host** (limitations vs SaaS vendeur). **Politique Video-AI** : **souveraineté** — **pas** d’usage de **Trigger.dev Cloud** ; API et CLI ciblent **uniquement** notre instance. Le monorepo avait mélangé **API HTTP** et **code des tasks** sous `apps/api`, source de confusion avec la **plateforme** Trigger (webapp, workers, registry, Compose upstream).

## Decision

1. **`apps/trigger`** — **workspace / package tasks** uniquement : `trigger.config.ts`, `src/trigger/*`, scripts `dev` / `trigger:deploy`, deps SDK (+ `@trigger.dev/build` si requis par le flux de build/deploy validé). CLI **toujours** `-a` vers **notre** instance.
2. **`infra/trigger-hosting`** — documentation Ops pour **référencer** la plateforme **upstream** (`github.com/triggerdotdev/trigger.dev`, `hosting/docker`) ; le monorepo **ne copie pas** la plateforme.
3. **`apps/api`** — client HTTP : `tasks.trigger()` avec **import de types uniquement** depuis le package `trigger` ; **bundle runtime sans code des tasks** ; **`TRIGGER_API_URL` + `TRIGGER_SECRET_KEY` obligatoires** pour `POST /jobs/render-pipeline` — **uniquement** instance **self-host** ; **pas de Trigger.dev Cloud** pour Video-AI.

## Consequences

### Positive

- Séparation claire **plateforme** (upstream / infra doc) vs **package tasks** (monorepo).
- Alignement avec la doc Trigger ([Triggering](https://trigger.dev/docs/triggering)) pour déclencher depuis un service séparé.
- Image API plus simple à raisonner (pas de logique worker embarquée).

### Negative

- Deux emplacements à maintenir (`apps/api`, `apps/trigger`) et CLI à lancer depuis **`apps/trigger`**.
- Validation end-to-end dépend du déploiement **upstream** (Coolify, registry, réseau Docker).

## References

- [infra/trigger-hosting](../../../infra/trigger-hosting/README.md) (repo root)
- [video-ai-orchestrator-decision](../reference/video-ai-orchestrator-decision.md)
