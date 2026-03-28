---
title: "Video-AI — Décision orchestrateur (Trigger.dev v4 vs Inngest)"
type: documentation
diataxis: explanation
status: published
area: video-ai
tags:
  - video-ai
  - orchestration
  - trigger-dev
  - inngest
  - adr
  - coolify
created: 2026-03-27
updated: 2026-03-27
related:
  - "[[reference/video-ai-upper-layers-mastra-openclaw]]"
  - "[[02-video-ai-roadmap]]"
  - "[[research/workflow-tools-synthesis]]"
  - "[[research/inngest]]"
  - "[[research/trigger-dev]]"
  - "[[runbooks/deploy-selfhost-api-frontend]]"
---

# Video-AI — Choix d’orchestrateur : Trigger.dev v4

**Décision** : **Trigger.dev v4** comme **seul** orchestrateur du cœur du pipeline (pré-v2 → v2). Inngest reste documenté en recherche mais n’est **pas** retenu pour l’implémentation actuelle.

**Périmètre** : monorepo Video-AI (Turborepo, Bun, Hono, Remotion, PostgreSQL), déploiement **self-hostable** (limitations documentées vs cloud managé) visé (**Coolify** / VPS), POC puis boucle **feedback → re-render** (v2).

**Note méthodologique** : ce document consolide une **recherche externe** (Perplexity / Deep Research, mars 2026). Les points **factuels** (versions, limites exactes, tarifs) doivent être **recoupés** avec la documentation officielle Inngest / Trigger.dev **pendant le spike** ; seules la **structure de décision** et les **critères** font foi jusqu’à validation terrain.

---

## Synthèse exécutive

Pour un pipeline de rendu vidéo pédagogique (Remotion) en déploiement **self-hostable** sur VPS/Coolify, avec stack **Turborepo + Bun + Hono** et une boucle **feedback → re-render** en v2, **Trigger.dev v4** est retenu : support **Bun** documenté et maintenu, modèle d’exécution adapté aux **tâches long-running** (workers persistants, pas de timeout de step obligatoire), stack **Compose upstream** avec **limitations documentées vs cloud** (warm start, autoscaling, checkpoints CRIU, etc.), intégration **Turborepo** documentée, et trajectoire produit alignée **vidéo / traitement lourd**.

**Inngest** reste pertinent en **serverless-first** (HTTP par step, excellents free tiers cloud) ; pour notre profil, les frictions viennent du couple **long-running + self-host** : l’option **Connect** (WebSocket, steps longs) n’est **pas** considérée comme disponible en self-host au moment de la recherche — à revalider sur la doc Inngest au spike.

---

## Tableau comparatif (synthèse)

| Critère | Inngest | Trigger.dev v4 |
|--------|---------|----------------|
| **Bun** | SDK + adapter Hono (`inngest/hono`, JSR/npm) ; connect ≥ Bun 1.1 | `runtime: "bun"` dans `trigger.config.ts` ; guide Bun dédié |
| **Hono** | Adapter officiel | Adapter Hono (cross-runtime : Bun, Deno, Workers, etc.) |
| **Long-running / render** | Modèle serverless (step = invocation HTTP) ; timeouts step (plafond ~2h sur offres payantes cloud) ; **Connect** pour dépasser — **self-host Connect** à vérifier / souvent « en développement » selon recherche | Workers **Docker** persistants ; timeout optionnel (`maxDuration`) ; checkpoints CRIU **cloud only** |
| **Retries / idempotence** | `step.run()` mémoïsé, retries / throttle configurables | Retry par task ; idempotence par design |
| **Self-host** | `inngest start` en beta ; Compose Postgres + Redis ; dashboard sans auth native | Compose v4 ; registry / object storage intégrés (selon doc) ; Helm K8s |
| **Monorepo Turborepo** | Pas d’exemple officiel Turborepo mis en avant | Guides Turborepo (tasks dans une app dédiée ou `packages/tasks`) — Video-AI : **`apps/trigger`** (package tasks), **`apps/api`** déclenche via SDK |
| **Observabilité** | Dashboard, traces, replay | Dashboard, TRQL / métriques, replay payload, logs, OTel |
| **Pricing cloud** | Free généreux en runs (ordre 50k/mois selon recherche) | Crédit / runs plus limités en free ; facturation **compute** à la seconde |
| **Lock-in** | SDK npm | SDK OSS MIT ; self-host proche du cloud (sauf features listées) |
| **Vidéo** | Cas Banger.Show + Remotion (file d’attente custom) | Doc **FFmpeg / video processing** explicitement |

---

## Analyse par critère (détail)

### 1. Compatibilité Bun + Hono

Les deux écosystèmes ciblent **Hono** et **Bun**. Trigger.dev v4 formalise **Bun** comme runtime de déploiement (ex. Bun 1.3.x, changelog actif). Inngest fournit l’adapter **Hono** et des exemples **Bun** ; des frictions ponctuelles CLI/dev-server peuvent exister — à valider en spike.

**Verdict** : les deux sont **candidats** sur Bun + Hono ; Trigger.dev est retenu pour les **autres** critères (self-host + long-running).

### 2. Modèle d’exécution (Remotion / `renderMedia` / CLI)

Le rendu Remotion peut durer **plusieurs minutes**. Inngest historique = **HTTP par step** → contraintes de timeout ; **Connect** compense en cloud mais pose question en **self-host**. Trigger.dev v4 = **container worker** → modèle naturel pour `remotion render` / `renderMedia()` sur une durée longue, sans ramener tout le graphe en HTTP court.

**Verdict** : **Trigger.dev** préféré pour **self-host + render long**, sous réserve de valider l’image worker (Chrome, FFmpeg, taille, cold start).

### 3. Self-host

Recherche : Inngest self-host **beta**, dashboard **sans auth** native, Connect self-host **non aligné** avec le besoin long-running. Trigger.dev v4 : Compose **GA** upstream, registry intégré, **self-hostable** avec écarts documentés vs cloud (warm starts, autoscaling, checkpoints CRIU, etc.).

**Verdict** : **Trigger.dev** retenu pour **maturité perçue** self-host + Coolify (voir annexe).

### 4. DX & Turborepo

Patterns documentés Trigger.dev : tâches dans une app dédiée ou **`packages/tasks`**. Video-AI : **`apps/trigger`** (workspace / package tasks) + **`apps/api`** comme client HTTP (`tasks.trigger`, types uniquement). CLI `init --runtime bun` ; **`login -a`** vers l’instance **self-hostable**. Inngest : `serve()` dans l’app ou process **connect** — intégration simple mais moins de **patterns Turborepo** officiels.

### 5. Observabilité

Les deux offrent dashboard et replay ; Trigger.dev met l’accent sur **requêtes / métriques / runs** (TRQL, etc.) — utile pour un pipeline render.

### 6. Pricing & vendor

- **Cloud** : comparer **runs** (Inngest) vs **compute** (Trigger) selon durée moyenne de render.
- **Self-host** : coût principal = **infra** ; les deux peuvent être **OSS** côté logiciel.

### 7. Cas d’usage vidéo

Les deux ont des **précédents** ; Trigger.dev cite explicitement **FFmpeg / video** dans la doc produit — critère secondaire mais rassurant.

---

## Décision : Trigger.dev v4

### Justification (points clés)

1. **Bun** comme runtime de déploiement **documenté** pour les workers.
2. **Workers persistants** (Docker) : adaptés à **render long** sans s’appuyer sur Connect côté Inngest en self-host.
3. **Self-host v4** : Compose / Helm, registry intégré — aligné **VPS / Coolify**.
4. **Turborepo** : patterns officiels + `init --runtime bun`.
5. **Observabilité** : runs, replay, logs — adapté au debug pipeline.
6. **Vidéo / FFMPEG** mentionné dans la doc produit.
7. **SDK OSS** (MIT) et déploiement **self-hostable** avec limitations documentées vs cloud.
8. **Workers sur infra** : pas uniquement « tout en HTTP public » pour chaque step long (selon modèle Trigger).
9. **v2** : enchaînements type **batch / wait** entre tâches sans second orchestrateur.
10. **Facturation cloud** : au **temps de compute** — cohérent avec rendus variables (si usage cloud un jour).

### Conditions de révision

| Si… | Alors… |
|-----|--------|
| Cible **uniquement** serverless (Vercel, Workers, pas de conteneur worker) | Réévaluer **Inngest** (modèle HTTP / step très adapté). |
| **Inngest Connect self-host** devient **GA** et stable pour steps longs | Réouvrir comparaison **long-running self-host**. |
| Renders **> 1 h** systématiques | Envisager **découpage** / chunks — les deux stacks ont des limites (CRIU self-host Trigger non disponible, etc.). |
| **Budget cloud** très serré avec **très nombreux** runs **courts** | Comparer **gratuits** Inngest vs Trigger sur la grille tarifaire **à jour**. |

---

## Architecture cible (POC)

```text
monorepo/
├── apps/
│   ├── api/                 # Hono + Bun, Drizzle ; déclenche runs (tasks.trigger), pas le code des tasks
│   ├── trigger/             # workspace / package tasks : trigger.config.ts + src/trigger/*
│   └── remotion/            # Studio + bundle
├── infra/
│   └── trigger-hosting/     # doc : référencer plateforme upstream (hosting/docker), pas copier Trigger.dev
└── (Coolify / VPS)          # Compose upstream webapp + worker + registry — hors racine Video-AI comme build context
```

**Flux POC cible** :

```text
API Hono → trigger render task → queue Trigger.dev
  → prepare (params, bundle check)
  → render (CLI ou renderMedia dans worker)
  → notify (webhook / mise à jour DB / fichier)
```

---

## Plan de spike (incl. Coolify)

### Étape 0 — Coolify (~1 h)

- **Option A** : service **catalogue Coolify** « Trigger.dev » (PostgreSQL managé ou externe via `DATABASE_URL`).
- **Option B** : Compose **community** type `coolify-trigger-v4` (Redis, ClickHouse, registry MinIO, etc. selon template).
- **Post-déploiement** :
  - **`DOCKER_RUNNER_NETWORKS`** = ID du réseau Docker Coolify (sinon les **containers de task** ne joignent pas le supervisor — point critique).
  - Exposer **dashboard** (ex. `:3000`) et **registry** (ex. `:5000`) si Compose custom.
  - Variables **registry** : `REGISTRY_HOST`, `TRIGGER_DOCKER_USERNAME`, `TRIGGER_DOCKER_PASSWORD` si besoin.
- **Valider** : dashboard OK + **task stub** exécutée.

### Étape 1 — Plateforme self-hostable Trigger.dev (local ou VPS)

Compose officiel **upstream** (`hosting/docker`), worker connecté — voir [`infra/trigger-hosting`](../../../infra/trigger-hosting/README.md).

### Étape 2 — Stub tasks dans **`apps/trigger`**

`init --runtime bun`, tasks pipeline (stub), **`trigger dev` / `deploy`** depuis **`apps/trigger`** avec **`login -a`** vers l’instance ; déclenchement **POST** Hono `/jobs/render-pipeline`, run visible dans le dashboard.

### Étape 3 — Spike **render** Remotion dans le worker (critique)

- `renderMedia()` (Node) **ou** `remotion render` (CLI).
- Image : **Chrome + FFmpeg** (`npx remotion browser ensure`, etc.), **taille** et **cold start**.
- Render court (**< 30 s**) jusqu’au fichier accessible.

### Étape 4 — Contrats partagés

Types `RenderJobPayload` / statut dans `@repo/contracts` (ou package dédié), endpoints Hono typés.

### Étape 5 — Observabilité & fin de run

Logs structurés, webhook fin de render, **replay** dashboard avec payload modifié.

---

## Risques et inconnues (spike)

| Risque | Impact | Mitigation |
|--------|--------|------------|
| Image worker **lourde** (Chrome + FFmpeg + deps) | Cold start, CI | Image de base Remotion officielle ; mesurer ~1,5–2 Go |
| **renderMedia** vs **CLI** dans le worker | Complexité | Préférer API programmatique si callbacks / progression |
| **Bun** vs deps Remotion / OTel | Compat | Basculer worker **`runtime: "node-22"`** si nécessaire |
| **Registry** self-host / Coolify | Déploi tasks | Registry intégré v4 + creds documentés |
| Renders **très longs** | Timeouts infra | `maxDuration`, découpage séquences |
| Affirmations **tarif / GA** des vendors | Décision erronée | Recheck **docs + pricing** au moment du spike |

---

## Annexe — Coolify : ce qui change concrètement

- Coolify peut déployer Trigger.dev via **one-click** ou **Compose** ; l’enjeu n’est pas « compatible ou non », mais **réseau Docker** + **registry** + **ports exposés**.
- **`DOCKER_RUNNER_NETWORKS`** : à renseigner après premier déploiement (réseau auto-généré Coolify) pour que le **supervisor** lance des tasks joignables.
- **Registry** : exposer et configurer les variables d’authentification pour que le **build/push** d’image de task fonctionne.
- **Inngest** sur Coolify : retours communautaires souvent « expérimental » (dashboard sans auth, maturité self-host moindre) — cohérent avec le choix Trigger pour **notre** profil, à nuancer si la doc Inngest a évolué.
- **Worker render Remotion** : probablement un **service Coolify séparé** (image dédiée Chrome/FFmpeg) en plus du stack Trigger — à figer au spike.

---

## Prochaines étapes (repo)

1. ~~Rédiger [`video-ai-rendering.md`](../runbooks/video-ai-rendering.md)~~ — **fait** ; itérer après choix runtime final.
2. ~~Mettre à jour [`02-video-ai-roadmap`](../02-video-ai-roadmap.md)~~ — **fait** (pré-v2 + POC stub). Poursuivre quand render réel + Coolify ops seront validés.
3. Runbook spike : [`trigger-dev-coolify-spike.md`](../runbooks/trigger-dev-coolify-spike.md) — **fait** (procédure) ; **exécution** infra sur VPS restante.
4. Promouvoir après spike validé une fiche **`reference/tools/trigger-dev.md`** (optionnel) ou ADR — voir [[adr/2026-03-27-trigger-workspace-and-upstream-platform]].

---

## Voir aussi

- [video-ai-upper-layers-mastra-openclaw](video-ai-upper-layers-mastra-openclaw.md) — Mastra + OpenClaw **au-dessus** de Trigger (v2, poste auteur)
- [02-video-ai-roadmap](../02-video-ai-roadmap.md)
- [workflow-tools-synthesis](../research/workflow-tools-synthesis.md)
- [deploy-selfhost-api-frontend](../runbooks/deploy-selfhost-api-frontend.md)
- [inngest](../research/inngest.md) · [trigger-dev](../research/trigger-dev.md)
