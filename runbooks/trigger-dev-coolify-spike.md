---
title: "Runbook – Spike Trigger.dev v4 self-host sur Coolify"
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
updated: 2026-03-27
related:
  - "[[reference/video-ai-orchestrator-decision]]"
  - "[[runbooks/deploy-selfhost-api-frontend]]"
  - "[[runbooks/api]]"
  - "[[runbooks/video-ai-rendering]]"
---

# Runbook – Spike Trigger.dev v4 + Coolify

Objectif : déployer **Trigger.dev v4** en **self-host** sur une instance **Coolify** (ou équivalent Docker), puis valider qu’une **task** s’exécute (stub), **avant** d’y brancher le render Remotion lourd.

**Décision produit** : orchestrateur **Trigger.dev v4** — [video-ai-orchestrator-decision](../reference/video-ai-orchestrator-decision.md).  
**Détail pas à pas spike** (étape 0 Coolify) : [même doc § Plan de spike](../reference/video-ai-orchestrator-decision.md#plan-de-spike-incl-coolify).

---

## Prérequis

- Compte / projet Trigger.dev (cloud ou self-host) et variables `TRIGGER_SECRET_KEY`, `TRIGGER_PROJECT_REF` pour `apps/api`.
- Coolify avec accès Docker sur le VPS.
- Repo Video-AI ; pour le dev local : **Node.js** pour la CLI `npx trigger.dev@latest` (la CLI n’est pas officiellement sur Bun — voir [guide Bun Trigger](https://trigger.dev/docs/guides/frameworks/bun)).

---

## Option A — Service catalogue Coolify « Trigger.dev »

1. Dans Coolify, ajouter le service **Trigger.dev** depuis le catalogue (template officiel ou communautaire à jour v4).
2. Renseigner **PostgreSQL** : soit base managée par le template, soit **DATABASE_URL** vers votre instance existante (ex. même Postgres que l’API — isoler schémas / DB selon politique interne).
3. Déployer ; noter l’URL du **dashboard** (souvent port `3000` ou équivalent derrière le proxy Coolify).

---

## Option B — Docker Compose custom (v4)

1. Utiliser le **Compose officiel** Trigger.dev v4 (documentation Trigger self-host) ou un template **Coolify-ready** (ex. communauté ; vérifier la date et la compatibilité v4).
2. **Build context** : suivre la doc Trigger (souvent dépôt dédié ou stack fourni), **pas** la racine Video-AI seule, sauf si vous avez fusionné les stacks (avancé).

---

## Points critiques après premier déploiement

### 1. Réseau Docker — `DOCKER_RUNNER_NETWORKS`

Coolify génère un **réseau Docker** interne. Le **supervisor** Trigger qui lance les **containers de task** doit partager ce réseau, sinon les runs restent **pending** ou les workers ne sont pas joignables.

- Récupérer l’**ID ou le nom** du réseau après le premier déploiement (UI Coolify / `docker network ls`).
- Définir la variable d’environnement documentée par Trigger pour le runner, typiquement **`DOCKER_RUNNER_NETWORKS`** (nom exact à confirmer dans la doc version déployée).

### 2. Registry d’images de tasks

Trigger v4 **build** et **push** une image pour les tasks. En self-host, un **registry** local (ex. `:5000`) est souvent inclus.

- Exposer le port **registry** dans Coolify si besoin.
- Configurer `REGISTRY_HOST`, `TRIGGER_DOCKER_USERNAME`, `TRIGGER_DOCKER_PASSWORD` (ou équivalent) selon la doc self-host.

### 3. Ports exposés

- **Dashboard** web (ex. `3000`).
- **Registry** si séparé (ex. `5000`).

Vérifier les **healthchecks** et les **labels** proxy (HTTPS) Coolify.

---

## Validation du spike

1. Depuis la machine de dev (avec Node) : `npx trigger.dev@latest dev` pointant vers le projet `apps/api` (voir [api](api.md) § Trigger).
2. Ou déclencher `POST /jobs/render-pipeline` sur l’API avec un body JSON valide.
3. Dans le **dashboard** Trigger : run **réussi** pour `render-pipeline` (ou task hello world intermédiaire).

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
