---
title: "Runbook – Video-AI rendering (local & runtime cible)"
type: runbook
diataxis: how-to
status: published
area: video
tags:
  - runbook
  - remotion
  - render
  - video-ai
  - trigger-dev
created: 2026-03-27
updated: 2026-03-28
related:
  - "[[runbooks/remotion]]"
  - "[[runbooks/deploy-selfhost-api-frontend]]"
  - "[[runbooks/api]]"
  - "[[reference/video-ai-orchestrator-decision]]"
  - "[[02-video-ai-roadmap]]"
---

# Runbook – Video-AI rendering

Procédure canonique pour **produire un fichier vidéo** (ou une still) à partir des compositions Remotion du monorepo, **en local** d’abord, puis **où faire tourner le render** pour un pipeline orchestré (**Trigger.dev self-host**, CI, ou cible média type cloud ex. Lambda — **sans** Trigger.dev Cloud pour l’orchestration Video-AI).

**Prérequis** : [remotion](remotion.md) (Studio, structure `apps/remotion`), [monorepo](monorepo.md) (`bun install` à la racine).

---

## 1. Contexte

| Ressource | Rôle |
|-----------|------|
| `apps/remotion` | Point d’entrée CLI ; `remotion.config.ts` ; `src/remotion/Root.tsx` enregistre les `Composition` |
| Image Docker `apps/remotion/Dockerfile` | **Remotion Studio** (prévisualisation), **pas** un worker headless de render batch |
| Orchestration (pré-v2) | **Trigger.dev v4** — les tasks appellent le render sur un **runtime** dédié (voir §4) |

---

## 2. Render local (contrat d’équipe)

Toutes les commandes s’exécutent depuis la **racine du monorepo** ou depuis `apps/remotion` comme indiqué.

### 2.1 Répertoire de travail

- **Recommandé** : `cd apps/remotion` pour les chemins relatifs vers `out/` ci-dessous.
- Le bundle webpack est généré par la CLI au besoin ; `bun run build` dans `apps/remotion` exécute `remotionb bundle` (validation CI / Docker).

### 2.2 Dossier de sortie

- Créer un répertoire **hors Git** ou ignoré : par ex. `apps/remotion/out/` (ajouter à `.gitignore` si vous versionnez des tests locaux).
- Convention proposée : `out/renders/<compositionId>-<YYYYMMDD-HHMM>.mp4`.

### 2.3 Composition de test (POC)

Pour un render **court** et reproductible (smoke test, pas la série pédagogique complète) :

| Composition `id` | Usage |
|------------------|--------|
| `MyComp` | Démo minimale, durée courte — voir `Root.tsx` |
| `TextDemo` | Démo longue ; préférer `MyComp` pour un smoke rapide |

Vérifier les `id` exacts dans [`apps/remotion/src/remotion/Root.tsx`](../../../apps/remotion/src/remotion/Root.tsx).

### 2.4 Commandes

```bash
cd apps/remotion

# Still (rapide, pas d’encode vidéo complet)
bunx remotion still MyComp out/smoke.png --frame=0

# Vidéo (MP4 H.264)
bunx remotion render MyComp out/smoke.mp4 --codec=h264

# Avec qualité / overwrite (aligné sur remotion.config.ts)
bunx remotion render MyComp out/smoke.mp4 --codec=h264 --overwrite
```

*Variante* : selon l’environnement, `bunx remotion` ou l’alias projet `remotionb` — voir [remotion § Usage](remotion.md).

### 2.5 Chrome / FFmpeg

- Première exécution : la CLI peut télécharger **Chromium** (bundled Remotion). Voir [Remotion – Ensure browser](https://www.remotion.dev/docs/cli/).
- **FFmpeg** doit être disponible sur la machine (PATH) pour l’encodage final.

### 2.6 Validation manuelle

- [ ] Le fichier sort dans le chemin attendu.
- [ ] Taille > 0 ; lecture dans un lecteur vidéo pour `MyComp` / still.
- [ ] En cas d’échec : logs CLI, RAM, résolution (`width`/`height` dans `Root.tsx`).

---

## 3. Intégration catalogue / API (rappel)

- Les entrées catalogue (`videos`, `video_versions`) utilisent `composition_id` aligné sur l’`id` Remotion — voir [frontend](frontend.md) et `sceneRegistry`.
- Après render manuel, l’URL du fichier peut être poussée vers le stockage objet / CDN **hors scope** de ce runbook (procédure ops à documenter quand le bucket est choisi).

---

## 4. Où exécuter le render (runtime cible — pré-v2 / Trigger)

**Décision actuelle** : l’image Docker documentée pour `apps/remotion` sert **Studio**, pas un render headless en production.

| Option | Description | Quand |
|--------|-------------|--------|
| **A. Machine / runner avec Bun + deps** | Même commandes qu’en §2 sur un runner (self-host, VM) | POC rapide, faible volume |
| **B. Container worker dédié** | Image avec Node ou Bun, FFmpeg, `npx remotion browser ensure` (ou équivalent), repo ou artefact bundle | **Recommandé** pour alignement Trigger.dev worker Docker |
| **C. Remotion Lambda** | AWS ; à évaluer si scale / pas de gestion de workers | Volumétrie élevée, budget cloud |
| **D. Job CI** | GitHub Actions / autre : render sur tag ou nightly | Artefacts de release, pas interactif |

**Spike** : mesurer **taille d’image**, **cold start**, et **durée** pour `MyComp` puis une composition série 01. Extensions Trigger utiles ensuite : voir [ffmpeg / puppeteer](https://trigger.dev/docs/config/config-file) dans la doc Trigger (build extensions).

**Coolify** : le stack Trigger self-host est décrit dans [trigger-dev-coolify-spike](trigger-dev-coolify-spike.md) ; le worker de render peut être un **service séparé** qui consomme la même logique que §2.

---

## 5. Orchestration Trigger.dev (référence code)

- Tasks : **`apps/trigger/src/trigger/`** (workspace / package tasks) — pipeline `prepare` → `render` (stub ou réel) → `notify`. L’API déclenche via `tasks.trigger` sans embarquer ce code — voir [api](api.md).
- Déclenchement HTTP : `POST /jobs/render-pipeline` — voir [api](api.md).
- Secrets : **`TRIGGER_SECRET_KEY`**, **`TRIGGER_API_URL`** (obligatoires côté API pour les jobs), `TRIGGER_PROJECT_REF` (workspace tasks) — ne pas committer ; **pas de Trigger.dev Cloud** (instance self-host uniquement).

---

## 6. Rollback / annulation

- Render local : supprimer les fichiers sous `out/` ; pas d’effet de bord serveur.
- Run Trigger : annulation depuis le **dashboard** de **notre** instance self-host (run en cours).

---

## Voir aussi

- [remotion](remotion.md)
- [video-ai-development](video-ai-development.md)
- [video-ai-orchestrator-decision](../reference/video-ai-orchestrator-decision.md)
- [trigger-dev-coolify-spike](trigger-dev-coolify-spike.md)
