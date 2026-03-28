---
title: "Video-AI — Tests par palier (roadmap & escalade agentique)"
type: documentation
diataxis: how-to
status: published
area: video-ai
tags:
  - video-ai
  - testing
  - turbo
  - trigger-dev
created: 2026-03-30
updated: 2026-03-30
related:
  - "[[02-video-ai-roadmap]]"
  - "[[reference/video-ai-upper-layers-mastra-openclaw]]"
  - "[[runbooks/api]]"
  - "[[runbooks/monorepo]]"
---

# Video-AI — Tests par palier

**Objectif** : aligner les tests sur l’**avancement** ([02-video-ai-roadmap](../02-video-ai-roadmap.md)) et la **doctrine** d’escalade des droits agent ([§ Escalade des capacités agent (2026)](../reference/video-ai-upper-layers-mastra-openclaw.md#progressive-agent-capabilities)) — sans confondre « où on en est » et « quels droits l’agent a déjà ».

## Commandes

| Commande | Rôle |
|----------|------|
| `bun run test` (racine monorepo) | `turbo run test` — packages qui exposent un script `test` (`api`, `frontend`, `@repo/contracts`, `trigger`). |
| `cd apps/api && bun test` | API seule (Bun) — pas besoin de `bun run dev` pour la plupart des cas. |
| `cd apps/frontend && bun run test` | Frontend (Vitest). |

Les tests **unitaires / contrats** ne nécessitent **pas** un worker Trigger ni la base Postgres pour les routes qui mockent le client Trigger ou tolèrent l’absence de DB (ex. `/videos` peut répondre 500 si `DATABASE_URL` manque — à durcir plus tard avec DB de test).

## Cartographie roadmap → périmètre de test

| Palier / phase | Focus test (résumé) | Emplacement / notes |
|----------------|---------------------|---------------------|
| **Palier 1** — Trigger seul, pas d’agent | Schéma payload pipeline ; garde-fous `POST /jobs/render-pipeline` ; **202** avec `triggerRenderPipeline` mocké ; ids des tasks `apps/trigger` | `@repo/contracts` (`RenderPipelinePayloadSchema`), `apps/api/test`, `apps/trigger/test` |
| **Niveau 3** — rendu hors stub | *(Quand implémenté)* test **slow** ou job CI avec image worker — à documenter ici | — |
| **v1.1** — feedback | *(Quand routes existent)* API GET/POST commentaires, contrats Zod, auth | `apps/api/test`, `@repo/contracts` |
| **v2** — agent read-only / diff | Mock LLM, assertions sur **sorties Zod** ; pas de tests « writer repo » avant garde-fous | TBD |

## Fichiers utiles

- API : [`apps/api/test/routes.test.ts`](../../../apps/api/test/routes.test.ts), [`apps/api/test/jobs.render-pipeline.mocked.test.ts`](../../../apps/api/test/jobs.render-pipeline.mocked.test.ts)
- Trigger client injectable : [`apps/api/src/lib/triggerRenderPipeline.ts`](../../../apps/api/src/lib/triggerRenderPipeline.ts)
- Contrats : [`packages/contracts/test/render-pipeline-payload.test.ts`](../../../packages/contracts/test/render-pipeline-payload.test.ts)
- Tasks : [`apps/trigger/test/renderPipeline.test.ts`](../../../apps/trigger/test/renderPipeline.test.ts)

## CI monorepo racine

La checklist roadmap **CI/CD sur le repo Video-AI racine** peut inclure `bun run test` comme gate — voir [02-video-ai-roadmap § v1](../02-video-ai-roadmap.md) (non livré au moment de la rédaction de ce runbook).

## Voir aussi

- [api](./api.md) — variables `TRIGGER_*`, `POST /jobs/render-pipeline`
- [monorepo](./monorepo.md) — Turborepo
