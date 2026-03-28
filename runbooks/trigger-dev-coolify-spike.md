---
title: "Runbook – Spike Trigger.dev v4 self-hostable sur Coolify"
type: runbook
diataxis: how-to
status: published
area: devops
tags:
  - runbook
  - trigger-dev
  - coolify
  - docker
created: 2026-03-27
updated: 2026-03-28
related:
  - "[[reference/video-ai-orchestrator-decision]]"
  - "[[runbooks/deploy-selfhost-api-frontend]]"
  - "[[runbooks/api]]"
  - "[[runbooks/video-ai-rendering]]"
---

# Runbook – Spike Trigger.dev v4 + Coolify

Objectif : déployer la **plateforme** Trigger.dev v4 (**self-hostable**, avec **limitations documentées vs cloud managé**) sur une instance **Coolify** (ou équivalent Docker), puis valider qu’une **task** s’exécute (stub), **avant** d’y brancher le render Remotion lourd.

**Vocabulaire** :

- **Plateforme** (webapp, supervisor, registry, DB, Compose) = **repo upstream** [`trigger.dev/hosting/docker`](https://github.com/triggerdotdev/trigger.dev) — **pas** le dossier `apps/trigger` du monorepo.
- **`apps/trigger`** = **workspace / package tasks** uniquement (code des tasks + `trigger.config.ts` + CLI `dev` / `deploy`).

**Décision produit** : orchestrateur **Trigger.dev v4** — [video-ai-orchestrator-decision](../reference/video-ai-orchestrator-decision.md).  
**Référence clone / SHA / checklist validation** : [`infra/trigger-hosting`](../../../infra/trigger-hosting/README.md).  
**Détail pas à pas spike** (étape 0 Coolify) : [même doc § Plan de spike](../reference/video-ai-orchestrator-decision.md#plan-de-spike-incl-coolify).

---

## Prérequis

- Instance **self-hostable** Trigger v4 (Compose upstream) accessible — **obligatoire** pour Video-AI ; **pas** de projet Trigger.dev Cloud dans le périmètre produit.
- Variables pour **l’API** Video-AI : **`TRIGGER_SECRET_KEY`** et **`TRIGGER_API_URL`** (tous deux **requis** pour `POST /jobs/render-pipeline`), et pour le **workspace tasks** : `TRIGGER_PROJECT_REF` dans `apps/trigger`.
- Coolify avec accès Docker sur le VPS.
- Repo Video-AI ; pour le dev local : **Node.js** pour la CLI `npx trigger.dev@latest` (la CLI n’est pas officiellement sur Bun — voir [guide Bun Trigger](https://trigger.dev/docs/guides/frameworks/bun)).
- CLI : toujours **`login -a` / `--api-url`** vers **notre** instance self-host — [doc self-host Docker § CLI](https://trigger.dev/docs/self-hosting/docker#cli-usage). Ne pas utiliser le cloud Trigger pour ce repo.

---

## Option A — Service catalogue Coolify « Trigger.dev »

1. Dans Coolify, ajouter le service **Trigger.dev** depuis le catalogue (template officiel ou communautaire à jour v4).
2. Renseigner **PostgreSQL** : soit base managée par le template, soit **DATABASE_URL** vers votre instance existante (ex. même Postgres que l’API — isoler schémas / DB selon politique interne).
3. Déployer ; noter l’URL du **dashboard** (souvent derrière le proxy Coolify ; port interne selon template — la doc upstream utilise souvent **8030** en local).

---

## Option B — Docker Compose upstream (v4)

1. Utiliser le **Compose officiel** dans **github.com/triggerdotdev/trigger.dev** sous `hosting/docker` ([documentation](https://trigger.dev/docs/self-hosting/docker)) — **build context = upstream**, pas la racine Video-AI.
2. Ne **pas** fusionner les stacks dans le monorepo sans processus de mise à jour clair ; **référencer** la plateforme depuis [`infra/trigger-hosting`](../../../infra/trigger-hosting/README.md).

---

## Points critiques après premier déploiement

### 1. Réseau Docker — `DOCKER_RUNNER_NETWORKS`

Coolify génère un **réseau Docker** interne. Le **supervisor** Trigger qui lance les **containers de task** doit partager ce réseau, sinon les runs restent **pending** ou les workers ne sont pas joignables.

- Récupérer l’**ID ou le nom** du réseau après le premier déploiement (UI Coolify / `docker network ls`).
- Définir la variable d’environnement documentée par Trigger pour le runner, typiquement **`DOCKER_RUNNER_NETWORKS`** (nom exact à confirmer dans la doc version déployée).

### 2. Registry d’images de tasks

Trigger v4 **build** et **push** une image pour les tasks. En self-hostable, un **registry** local (ex. `:5000`) est souvent inclus.

- Exposer le port **registry** dans Coolify si besoin.
- Configurer `REGISTRY_HOST`, `TRIGGER_DOCKER_USERNAME`, `TRIGGER_DOCKER_PASSWORD` (ou équivalent) selon la doc self-host.
- La machine qui exécute **`deploy`** doit pouvoir **`docker login`** vers ce registry.

### 3. Ports exposés

- **Webapp** / dashboard (selon template).
- **Registry** si séparé.

Vérifier les **healthchecks** et les **labels** proxy (HTTPS) Coolify.

---

## Validation du spike

1. Depuis la machine de dev (avec Node) : `npx trigger.dev@latest login -a <URL-instance>` puis **`bun run trigger:dev`** ou **`bun run trigger:deploy`** depuis **`apps/trigger`** (voir [api](api.md) § Trigger).
2. Ou déclencher `POST /jobs/render-pipeline` sur l’API avec un body JSON valide (`TRIGGER_API_URL` + `TRIGGER_SECRET_KEY` configurés sur l’API).
3. Dans le **dashboard** de **votre** instance : run **réussi** pour `render-pipeline` (ou task hello world intermédiaire).

Checklist étendue : [`infra/trigger-hosting` § Validation](../../../infra/trigger-hosting/README.md#validation-checklist-real-environment).

---

## Prochaines étapes (hors ce spike)

- Worker **render** Remotion : image avec Chrome + FFmpeg — [video-ai-rendering](video-ai-rendering.md) §4.
- **Secrets** : injecter `TRIGGER_*` via **Coolify Secrets**, jamais dans le dépôt.

---

## Rollback / annulation

- Arrêter le service Coolify Trigger ; les runs en cours se terminent ou échouent selon timeout.
- Conserver **exports DB** Trigger si vous devez réinstaller (selon doc Trigger backup).

---

## Voir aussi

- [deploy-selfhost-api-frontend](deploy-selfhost-api-frontend.md)
- [video-ai-orchestrator-decision](../reference/video-ai-orchestrator-decision.md)
- [infra/trigger-hosting](../../../infra/trigger-hosting/README.md)
