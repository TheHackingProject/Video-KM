---
title: "Video-AI — Roadmap & état des lieux"
type: documentation
diataxis: reference
status: published
area: video-ai
tags:
  - video-ai
  - roadmap
  - planning
  - v1
  - v2
created: 2026-03-25
updated: 2026-03-27
related:
  - "[[01-index]]"
  - "[[reference/video-ai-orchestrator-decision]]"
  - "[[reference/video-ai-upper-layers-mastra-openclaw]]"
  - "[[explanation/video-ai-vision]]"
  - "[[reference/video-lifecycle]]"
  - "[[research/workflow-tools-synthesis]]"
  - "[[runbooks/video-ai-development]]"
  - "[[video-ai-preparation/serie-01-git-github]]"
---

# Video-AI — Roadmap & état des lieux

**Emplacement** : racine `KM/Docs`, fichier **`02-…`** — suite logique de [`01-index`](01-index.md) pour **où en est le plan** sans ouvrir l’index.

**Objectif** : vue **dev** complète — **cases à cocher**, **suites de versions**, **ordre de priorité** aligné sur les décisions **Trigger.dev v4** ([orchestrateur](./reference/video-ai-orchestrator-decision.md)) et **Mastra / OpenClaw** ([couches supérieures](./reference/video-ai-upper-layers-mastra-openclaw.md)).  
**À mettre à jour** après chaque jalon : cocher/décocher, bump `updated`.

## Légende

| Symbole | Signification |
|---------|----------------|
| `[x]` | **Livré** dans le dépôt (code ou doc présent, utilisable). |
| `[ ]` | **Pas fait** ou absent du repo. |
| *Partiel* | Sous-points : ce qui manque pour fermer le volet. |

*Les chemins sont relatifs à la racine du monorepo `Video-AI/` sauf mention `KM/Docs/`.*

---

## Cartographie doc (ne pas dupliquer ailleurs)

| Document | Rôle |
|----------|------|
| [video-ai-vision](./explanation/video-ai-vision.md) | **Pourquoi** et trajectoire **v1 / v2 / v3**. |
| [video-lifecycle](./reference/video-lifecycle.md) | **Séquence** canonique + [§ Platform roadmap v1.1](./reference/video-lifecycle.md#platform-roadmap-v11-feedback). |
| [workflow-tools-synthesis](./research/workflow-tools-synthesis.md) | **Inngest / Trigger.dev / Mastra / OpenClaw** — rôles, **ordre d’intégration** (§3). |
| [video-ai-orchestrator-decision](./reference/video-ai-orchestrator-decision.md) | **Décision** : **Trigger.dev v4** vs Inngest + spike **Coolify** (réseau Docker, registry). |
| [video-ai-upper-layers-mastra-openclaw](./reference/video-ai-upper-layers-mastra-openclaw.md) | **Mastra** (LLM dans tasks Trigger) + **OpenClaw** (poste auteur), HITL, ordre v1.1 → v2. |
| [video-ai-preparation](./video-ai-preparation/video-ai-preparation.md) | Formats, template pilot, **avant** code. |
| [serie-01-git-github](./video-ai-preparation/serie-01-git-github.md) | **Ordre** des clips série 01 + liens outlines. |
| [video-ai-development](./runbooks/video-ai-development.md) | Workflow quotidien, Studio, qualité. |
| [remotion](./runbooks/remotion.md) · [api](./runbooks/api.md) · [frontend](./runbooks/frontend.md) · [postgres-local](./runbooks/postgres-local.md) | Procédures par couche. |
| [deploy-selfhost-api-frontend](./runbooks/deploy-selfhost-api-frontend.md) | Docker / Coolify / VPS. |
| [soul-recherche-visuelle](./research/soul-recherche-visuelle.md) · [git-github-vulgarisation-visuelle](./research/git-github-vulgarisation-visuelle.md) | Recherche visuelle / vulgarisation. |

---

## Synthèse par phase (vision produit)

| Phase | Cible | Statut dev (résumé) |
|-------|--------|---------------------|
| **v1** | Vidéos Remotion, équipe autonome, rendu manuel/CLI, monorepo stable | **~75–85 %** — cœur livré ; série 01 incomplète ; runbook **rendering** absent ; **pas de CI** sur le monorepo racine. |
| **v1.1** | Catalogue + **feedback** (threads, commentaires timecodés) | **Schéma DB + API lecture vidéos** ; **pas** d’API commentaires ni UI feedback. |
| **Pré-v2** | Runbook rendu distant + **POC** orchestrateur | **Décision** : [Trigger.dev v4](./reference/video-ai-orchestrator-decision.md) — pas encore de deps ni POC ; pas de `video-ai-rendering.md`. |
| **v2** | Boucle feedback → IA → re-render | **Architecture doc** : [couches supérieures](./reference/video-ai-upper-layers-mastra-openclaw.md) (Mastra **dans** Trigger, HITL, OpenClaw local) ; **zéro impl** pipeline. |
| **v3** | Régénération guidée | Vision uniquement. |

---

## Ordre de priorité global (ce qu’on fait avant quoi)

*Ordre validé pour enchaîner sans sur-ingénierie : rendu et données feedback avant IA lourde.*

| # | Priorité | Pourquoi |
|---|----------|----------|
| **1** | **Série 01 pédagogie** (outlines + comps **05+**) | Finir le cœur **v1** métier avant d’industrialiser le rendu. |
| **2** | **Runbook** [`video-ai-rendering.md`](./runbooks/video-ai-rendering.md) | Contrat d’équipe sur `remotion render` (local → artefact → stockage) ; **prérequis honnête** avant pré-v2. |
| **3** | **CI minimale** (racine monorepo) | Optionnel en parallèle du 1–2 ; sécurise les PR avant d’ajouter Trigger. |
| **4** | **v1.1 feedback** (API + contrats + UI + auth/modération) | Données réelles de feedback → base pour toute analyse LLM ; **Mastra peu pertinent avant** (cf. [couches supérieures](./reference/video-ai-upper-layers-mastra-openclaw.md#e-ordre-dintroduction)). |
| **5** | **Pré-v2 — Trigger.dev v4** | Spike **Coolify** (`DOCKER_RUNNER_NETWORKS`, registry, ports) puis deps, task stub, **render** dans worker (Chrome/FFmpeg) — [décision](./reference/video-ai-orchestrator-decision.md). **Pas de Mastra** au POC orchestration. |
| **6** | **Runbook OpenClaw** [`openclaw-permissions.md`](./runbooks/openclaw-permissions.md) | Formaliser permissions poste auteur ; peut démarrer **en parallèle** des phases 1–4 (hors prod). |
| **7** | **v2** | Task pipeline feedback → (LLM direct puis **Mastra** si besoin mémoire/multi-agents) → **HITL** Trigger → re-render — [plan POC 5 étapes](./reference/video-ai-upper-layers-mastra-openclaw.md#e-ordre-dintroduction). |

**Chemin critique** (simplifié) :

```mermaid
flowchart TB
  pedago[v1_pedagogie_serie01]
  rendering[runbook_video_ai_rendering]
  feedback[v1_1_feedback_API]
  trigger[pre_v2_Trigger_Coolify_worker]
  ia[v2_Mastra_HITL_rerender]

  pedago --> rendering --> feedback --> trigger --> ia
```

*En parallèle du chemin critique* : **CI** monorepo (dès que possible) ; runbook **OpenClaw** (poste auteur).

---

## v1 — état détaillé (checklist dev)

### Monorepo, tooling, déploiement doc

- [x] Turborepo, workspaces `apps/*`, `packages/*`
- [x] Bun, Biome, TypeScript partagé (`@repo/typescript-config`, `@repo/eslint-config`)
- [x] Runbooks : [monorepo](./runbooks/monorepo.md), [bun-biome](./runbooks/bun-biome.md), [dependencies-submodules](./runbooks/dependencies-submodules.md)
- [x] [deploy-selfhost-api-frontend](./runbooks/deploy-selfhost-api-frontend.md) (Dockerfiles `apps/*`)
- [ ] **CI/CD sur le repo Video-AI racine** (pas de `.github/workflows` dédiés au monorepo ; workflows présents sous `KM/Course/*` uniquement)
- [x] Runbook [video-ai-rendering](./runbooks/video-ai-rendering.md) (local → artefact → runtime worker / Trigger)

### Apps Remotion

- [x] `apps/remotion` — Studio, compositions démos (`TextDemo`, `CodeDemo`, …) enregistrées dans [`Root.tsx`](../../apps/remotion/src/remotion/Root.tsx)
- [x] **Série 01** — shell partagé [`Serie01SceneShell.tsx`](../../apps/remotion/src/remotion/compositions/serie-01/Serie01SceneShell.tsx)
- [x] Pilotes **01–04** : composants + `Composition` dans `Root.tsx` (`Pilot01Prerequis` … `Pilot04Branch`)
- [ ] Pilotes **05–08** (Merge, PR, Fork, git diff optionnel) — ni outlines KM ni comps — voir [serie-01-git-github](./video-ai-preparation/serie-01-git-github.md)

### Bibliothèque UI / Remotion

- [x] `@repo/ui` (dont couche Remotion) — Storybook `apps/storybook`
- [x] `@repo/remotion-lib` — blocs réutilisables (couverture **partielle** ; enrichissement continu)
- [x] Skills agent : `thp-video-generation`, `thp-solarpunk-visual`, `remotion-best-practices` (voir [01-index § skills](./01-index.md#agent-skills-versioned-in-repo))

### Préparation contenu (KM)

- [x] [video-ai-preparation](./video-ai-preparation/video-ai-preparation.md) — formats, template, shortlist
- [x] Outlines série 01 : `pilot-01` … `pilot-04` dans `KM/Docs/video-ai-preparation/`
- [ ] Outlines **pilot-05+** (Merge, …) — *prochaine étape doc* : [serie-01-git-github § Prochaine](./video-ai-preparation/serie-01-git-github.md#prochaine-étape)

### Recherche & méthode (non bloquant v1)

- [x] [soul-recherche-visuelle](./research/soul-recherche-visuelle.md), [git-github-vulgarisation-visuelle](./research/git-github-vulgarisation-visuelle.md)
- [x] Fiches outils v2+ : [inngest](./research/inngest.md), [trigger-dev](./research/trigger-dev.md), [mastra](./research/mastra.md), [openclaw](./research/openclaw.md) + [synthèse](./research/workflow-tools-synthesis.md)
- [x] **Décisions architecture** : [orchestrateur Trigger.dev v4](./reference/video-ai-orchestrator-decision.md), [Mastra + OpenClaw](./reference/video-ai-upper-layers-mastra-openclaw.md) (impl code **pas** requise pour cocher cette ligne)

### Plateforme catalogue (API + frontend + DB)

- [x] **Schéma** [`packages/db/src/schema.ts`](../../packages/db/src/schema.ts) : `videos`, `video_versions`, `feedback_threads`, `feedback_comments` (dont `timestamp_seconds` pour timecode)
- [x] Migrations Drizzle + seed + [postgres-local](./runbooks/postgres-local.md)
- [x] API Hono [`apps/api/src/routes/videos.ts`](../../apps/api/src/routes/videos.ts) : `GET /videos`, `GET /videos/:slug` + contrats `@repo/contracts`
- [x] Tests minimaux API ([`apps/api/test/routes.test.ts`](../../apps/api/test/routes.test.ts))
- [x] Frontend Vite : liste + détail vidéo, [`sceneRegistry`](../../apps/frontend/src/sceneRegistry.tsx) pour lecteur / métadonnées par `compositionId`
- [ ] **Intégration produit THP** (hors périmètre détaillé ici) — cf. [video-lifecycle](./reference/video-lifecycle.md)

---

## Série 01 — tableau clip par clip

Aligné sur [serie-01-git-github](./video-ai-preparation/serie-01-git-github.md).

| # | Clip | Outline KM | Composition Remotion | Fichiers repères |
|---|------|------------|----------------------|------------------|
| 1 | Pré-requis | [x] `pilot-01-prerequis-outline.md` | [x] `Pilot01Prerequis` | `serie-01/Pilot01Prerequis.tsx` |
| 2 | Git vs GitHub | [x] `pilot-02-git-vs-github-outline.md` | [x] `Pilot02GitVsGithub` | `Pilot02GitVsGithub.tsx` |
| 3 | Commit | [x] `pilot-03-commit-outline.md` | [x] `Pilot03Commit` | `Pilot03Commit.tsx` |
| 4 | Branch | [x] `pilot-04-branch-outline.md` | [x] `Pilot04Branch` | `Pilot04Branch.tsx` |
| 5 | Merge | [ ] | [ ] | *à créer* |
| 6 | Pull request (+ review) | [ ] | [ ] | *à créer* |
| 7 | Fork | [ ] | [ ] | *à créer* |
| 8 | git diff (optionnel) | [ ] | [ ] | *à créer* |

---

## v1.1 — suite prévue (feedback catalogue)

*Objectif* : exploiter les tables déjà migrées et exposer le flux « lire / écrire » feedback sans casser le catalogue v1.

**Réf.** : [video-lifecycle § Platform roadmap](./reference/video-lifecycle.md#platform-roadmap-v11-feedback).

**Lien v2 / IA** : sans API feedback et données réelles, peu d’intérêt à pousser **Mastra** ; la séquence recommandée est **v1.1 →** (option **LLM direct** dans une task) **→ Mastra** quand mémoire / multi-agents est justifié — [couches supérieures § E](./reference/video-ai-upper-layers-mastra-openclaw.md#e-ordre-dintroduction).

### Données & API

- [x] Tables `feedback_threads`, `feedback_comments` (+ FK `video_id` / `thread_id`)
- [ ] `GET /videos/:slug/comments` (ou équivalent REST cohérent avec les slugs)
- [ ] `POST /videos/:slug/comments` (création commentaire, optionnellement `timestamp_seconds`)
- [ ] Règles **auth** (qui peut poster ?) et **modération** (spam, contenu) — à trancher produit
- [ ] Validation / contrats dans `@repo/contracts` pour les payloads feedback

### Frontend & UX

- [ ] UI liste / fil de discussion par vidéo
- [ ] Saisie commentaire avec champ timecode optionnel (lié au player si lecteur intégré)
- [ ] Feature flag ou déploiement progressif si besoin

### Qualité

- [ ] Tests API + tests UI ciblés sur le flux feedback

---

## Pré-v2 — suite prévue (rendu & orchestration)

*Aligné sur* [workflow-tools-synthesis §3](./research/workflow-tools-synthesis.md#3-ordre-dintégration-recommandé) · orchestrateur **acté** : [video-ai-orchestrator-decision](./reference/video-ai-orchestrator-decision.md).

- [x] **Runbook** [video-ai-rendering](./runbooks/video-ai-rendering.md) : rendu local + § runtime (worker / Lambda / CI)
- [ ] **Design** rendu distant (choix final Lambda vs worker Docker) — à figer dans le runbook ou ADR après spike image
- [x] **Doc spike Coolify** : [trigger-dev-coolify-spike](./runbooks/trigger-dev-coolify-spike.md) — **infra** self-host (réseau `DOCKER_RUNNER_NETWORKS`, registry) : *à valider sur VPS*
- [x] **Code POC minimal** : `@trigger.dev/sdk` dans `apps/api`, tasks `prepare-render` → `render-video` (**stub**) → `notify-render`, `POST /jobs/render-pipeline` — voir [api](./runbooks/api.md) ; **render réel** (Chrome + FFmpeg) + prod Coolify : *suite*
- [x] **Décision orchestrateur** : [video-ai-orchestrator-decision](./reference/video-ai-orchestrator-decision.md) (**Trigger.dev v4**, pas Inngest)
- [x] **Décision couches IA (doc)** : [video-ai-upper-layers-mastra-openclaw](./reference/video-ai-upper-layers-mastra-openclaw.md) — **impl Mastra/OpenClaw = v2**, pas pré-v2
- [x] **Pas** de Mastra dans le POC orchestration (convention respectée dans le code livré)

---

## v2 — suite prévue (boucle feedback + IA)

**Réf. architecture couches** : [video-ai-upper-layers-mastra-openclaw](./reference/video-ai-upper-layers-mastra-openclaw.md) (Mastra dans Trigger, OpenClaw local, HITL, POC en 5 étapes).

- [ ] Ingestion feedback stable (prérequis v1.1)
- [ ] Orchestrateur (celui choisi en pré-v2) : file d’événements « feedback reçu / vidéo à rafraîchir »
- [ ] **Mastra** (ou équivalent) comme **étape** du graphe : priorisation, suggestions de script / scène — pas comme second orchestrateur ([synthèse](./research/workflow-tools-synthesis.md) · [couches supérieures](./reference/video-ai-upper-layers-mastra-openclaw.md))
- [ ] Re-rendu déclenché depuis le workflow + traçabilité (qui a validé quoi)
- [ ] **OpenClaw** : optionnel, poste auteur — jamais socle du pipeline central ; runbook dédié [openclaw-permissions.md](./runbooks/openclaw-permissions.md) *(à créer — gabarit dans [§ C couches supérieures](./reference/video-ai-upper-layers-mastra-openclaw.md#c-openclaw--poste-auteur))*

---

## v3 — rappel

Régénération **guidée**, garde-fous humains renforcés — détail dans [video-ai-vision](./explanation/video-ai-vision.md). Pas de checklist d’implémentation à ce stade.

---

## Priorités — rappel court (identique au tableau global)

1. Série 01 **05+** → 2. **`video-ai-rendering.md`** → 3. **CI** (parallèle OK) → 4. **v1.1 feedback** → 5. **Pré-v2 Trigger + Coolify + render worker** → 6. **`openclaw-permissions.md`** (parallèle OK) → 7. **v2** Mastra / HITL / re-render.

---

## Voir aussi

- [01-index](./01-index.md)
- [research/README](./research/README.md)
