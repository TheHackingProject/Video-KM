---
title: "Inngest"
type: research
diataxis: explanation
status: draft
last_reviewed:
area: tooling
tags:
  - research
  - inngest
created: 2026-03-18
updated: 2026-03-18
related:
  - "[[01-index]]"
  - "[[research/README]]"
---

# Inngest

## Summary

Inngest est une plateforme de **fonctions serverless événementielles et durables** (event‑driven workflows) qui permet d’écrire, versionner et superviser des workflows asynchrones complexes directement en code (TypeScript, etc.), avec :

- **fonctions multi‑étapes** (pause, reprise, attente d’événements pendant des heures/jours),
- **retries par étape**, gestion d’erreurs fine et observabilité intégrée,
- **déclenchement par événements** (webhooks, events d’app, CRON, API),
- intégration naturelle avec des stacks modernes (Next.js, edge/serverless, queues externes, services vidéo/IA…).

Référence officielle : `https://www.inngest.com` (docs et blog, notamment les articles sur les workflows vidéo et AI).

## État du projet Video‑AI (rappel ciblé)

- Projet organisé en **monorepo Turborepo** :
  - `apps/remotion` : Remotion Studio (création & preview des compositions),
  - `packages/ui` : design system statique,
  - `packages/remotion-lib` : primitives & blocs animés (cible),
  - documentation sous `KM/Docs` (architecture, vision, lifecycle).
- **Aujourd’hui** : le rendu vidéo est essentiellement **local et manuel** :
  - dev et preview via Remotion Studio local,
  - rendu via CLI (`remotion render ...`), pas de pipeline distant,
  - pas de backend ni de workers asynchrones dans ce repo.
- Vision moyen/long terme (docs `video-ai-vision` + `video-lifecycle`) :
  - industrialiser le cycle : idée → préparation → scènes/compositions → review → render → diffusion THP → feedback → itération,
  - v2/v3 : **boucle de feedback + régénération** plus ou moins automatisée,
  - futur **pipeline de rendu batch / serverless** (Lambda ou équivalent) encore à formaliser dans un runbook dédié.

Conséquence : pour l’instant, **aucun moteur de workflow** (Inngest, Trigger.dev, etc.) n’est utilisé ; tout ce qui concerne l’orchestration asynchrone est encore au stade “à définir”.

## Evaluation : fit avec Video‑AI

### Où Inngest pourrait apporter de la valeur

1. **Déclenchement du rendu vidéo (event‑driven)**
   - Événements possibles :
     - `video.render.requested` (création/modif d’un script ou d’une composition, merge d’une PR, action manuelle dans une app web),
     - `course.lesson.updated` / `course.published` côté THP,
     - demande explicite de “regénérer cette vidéo” après feedback.
   - Inngest gèrerait la **création fiable des jobs de rendu**, avec retries en cas d’échec (timeout Lambda, erreur réseau, quota, etc.).

2. **Pipeline de rendu multi‑étapes**
   - Exemple de workflow cible :
     1. `prepare_inputs` : calculer props, assets, prompts, metadata pour Remotion,
     2. `render_video` : lancer un rendu Remotion (local, Lambda ou autre worker),
     3. `post_process` : compression, thumbnails, génération de chapitres, éventuelle transcription,
     4. `publish_to_thp` : upload CDN / storage, mise à jour de la base de cours,
     5. `notify` : notifier l’équipe ou les outils internes qu’une nouvelle version est prête.
   - Chaque étape serait :
     - **retriable indépendamment**,
     - **observable** (logs, évènements, timeline),
     - configurable (timeouts, backoff).

3. **Boucle feedback → régénération (v2/v3)**
   - Événement `video.feedback.submitted` (depuis un outil interne ou la plateforme THP) :
     - tri et priorisation (peut impliquer de l’IA),
     - décision : re‑rendu automatique vs. demande de validation humaine,
     - déclenchement d’un nouveau workflow `video.regeneration.started`.
   - Inngest est bien adapté pour ces **workflows longue durée** (attente d’événements, révisions, validations).

4. **Planification / Cron**
   - Vérifications périodiques (cohérence cours ↔ vidéos, refresh de contenus),
   - rendus planifiés (ex : régénérer une série de vidéos après une mise à jour majeure des composants ou du style),
   - monitoring de “drift” entre la source de vérité (cours) et les vidéos générées.

### Avantages spécifiques pour Video‑AI

- **Modèle mental “event‑driven video lifecycle”** : colle très bien à la vision déjà décrite dans `video-lifecycle.md`.
- **Code‑first** : on reste dans TypeScript/JS, aligné avec Remotion et les apps web,
- **Gestion de la complexité asynchrone** : plutôt que recoder une couche maison de queue + state machine, on délègue à Inngest la partie “durable workflow”.
- **Observabilité intégrée** : utile pour une équipe pédagogique/vidéo qui a besoin de voir où en est chaque vidéo (en file d’attente, en rendu, publié, en erreur).

### Limites à court terme

- Tant que :
  - le rendu reste **local / artisanal**,
  - il n’y a **pas de backend ni de workers** déployés,
  - le volume de vidéos reste modeste,
  
  l’ajout d’Inngest serait surtout **de la complexité en avance de phase**.  
  Pour la v1 (studio + quelques vidéos pilotes), une orchestration simple (scripts CLI, quelques jobs manuels ou un cron minimal) peut suffire.

## Verdicte préliminaire

- **Utilité** :
  - **Court terme (v1, POC)** : faible à moyenne – Inngest n’apporte pas grand‑chose tant que tout tourne localement et que le volume reste faible.
  - **Moyen/long terme (v2/v3, prod THP)** : **élevée** – très bon candidat pour orchestrer :
    - le pipeline de rendu (multi‑étapes, tolérant aux pannes),
    - la boucle feedback → régénération,
    - les tâches planifiées.
- **Optimalité** :
  - Techniquement, Inngest est **bien aligné** avec un futur pipeline vidéo event‑driven (serverless, multi‑étapes, besoin de durabilité).
  - Mais il n’est **optimal que si** :
    - on assume un modèle d’architecture **event‑driven** (events partout : rendu demandé, feedback reçu, cours mis à jour…),
    - on prévoit un **backend / environnement serverless** hébergeant ces fonctions,
    - on accepte la dépendance à une plateforme tierce (ou on a un plan B si besoin de self‑host / autre provider).

Conclusion :  
> **Inngest est un très bon fit stratégique pour Video‑AI à partir du moment où l’on industrialise le rendu et le feedback loop (v2+). Pour la v1 studio + pilotes, il vaut mieux d’abord stabiliser le lifecycle, le format et le runbook de rendu, puis introduire Inngest comme “orchestrateur” plutôt que comme prérequis.**

## Comparaison / alternatives

### Trigger.dev

- Outil proche conceptuellement : workflows event‑driven, jobs durables, cron, intégration JS/TS.
- Points à comparer spécifiquement pour Video‑AI :
  - **expérience DX / intégration Next.js** (utile si `apps/web` devient une vraie app de pilotage vidéo),
  - **prix / limites de plans** selon le volume de jobs de rendu,
  - **latence inter‑étapes** et capacité à gérer des workflows longs (plusieurs heures/jours),
  - intégrations natives utiles (GitHub, queues, stockage, etc.).
- Sans benchmark détaillé, les deux sont **candidats sérieux** ; la décision devrait passer par un petit POC (voir section POC).

### Solutions “maison” (queues + workers custom)

- Exemple : Redis + worker Node + CRON maison.
- Avantage : contrôle total, pas de dépendance à une plateforme SaaS.
- Inconvénients :
  - on doit recréer la plupart des briques qu’Inngest fournit déjà : retries, backoff, replays, traces, dashboard, etc.,
  - plus de surface de maintenance pour une petite équipe.
- Dans le contexte Video‑AI (focus pédagogique + outils vidéo plutôt que plateforme infra), **reconstruire un orchestrateur complet n’est pas aligné** avec la vision KISS.

### Autres (OpenClaw, Mastra, etc.)

- Déjà en recherche dans d’autres fichiers (`openclaw.md`, `mastra.md`, `trigger-dev.md`).
- Pour l’instant, ils semblent :
  - plus orientés **AI agents / pipelines** (Mastra),
  - ou des solutions plus expérimentales / ciblées (OpenClaw),
  - moins “clé en main” pour un pipeline vidéo structuré et robuste que Inngest/Trigger.dev.

## Risks & constraints

- **Dépendance fournisseur** :
  - Lock‑in potentiel sur la plateforme Inngest (features spécifiques, stockage d’état, pricing),
  - nécessité de surveiller les coûts si le nombre de vidéos explose.
- **Complexité d’architecture** :
  - introduit tôt un paradigme event‑driven qu’il faudra assumer partout (et bien documenter),
  - risque d’ajouter une couche d’abstraction avant que les besoins soient bien stabilisés.
- **Infra manquante aujourd’hui** :
  - absence actuelle de backend / environnement serverless dans le repo Video‑AI,
  - besoin de définir un **runbook de rendu distant** (ex : Remotion Lambda, Workers, etc.) avant d’exploiter tout le potentiel d’Inngest.
- **Connaissance / montée en compétence** :
  - courbe d’apprentissage pour bien modéliser les workflows, les évènements, et éviter un “spaghetti d’events”.

## POC / notes

### POC recommandé (quand le rendu distant sera cadré)

Objectif : **prouver** qu’Inngest gère bien un pipeline de rendu vidéo typique Video‑AI avec observabilité et résilience, sans trop de friction DX.

Scénario minimal :

1. **Événement d’entrée**
   - `video.render.requested` émis depuis une app Next.js (future `apps/web`) ou un script CLI.

2. **Workflow Inngest**
   - Étape 1 : `prepare_inputs` – charger le “pilot outline” + composants Remotion → construire les props d’une composition.
   - Étape 2 : `render_video` – appeler un endpoint de rendu (local ou Lambda) pour produire le MP4.
   - Étape 3 : `upload_and_publish` – uploader vers un bucket (ou stockage THP de test), renvoyer une URL + metadata.
   - Étape 4 : `notify` – envoyer un message (Slack / mail / log structuré) avec le statut et le lien.

3. **Critères de succès**
   - retries corrects en cas d’échec de rendu,
   - visibilité claire de l’état de chaque run,
   - DX acceptable pour un dev Video‑AI (lecture/édition du workflow).

### Prochaine étape documentaire

- Une fois un choix fait (Inngest, Trigger.dev ou autre) :
  - créer une fiche dans `reference/tools/<outil>.md` avec :
    - “quand l’utiliser / quand éviter”,
    - patterns recommandés pour Video‑AI,
    - liens vers le runbook de rendu.
  - éventuellement créer un ADR dédié si l’outil est central pour l’architecture.

## References

- Site & docs Inngest : `https://www.inngest.com` (docs, guides multi‑step functions, patterns event‑driven).
- Article “event‑driven video processing workflow with Inngest” (exemple proche de Video‑AI, avec un pipeline upload → traitement multi‑services).
- `KM/Docs/reference/video-lifecycle.md` – cycle de vie vidéo cible.
- `KM/Docs/explanation/video-ai-vision.md` – vision v1/v2/v3.
- `KM/Docs/runbooks/video-ai-development.md` – runbook de dev.
- Futur : `KM/Docs/runbooks/video-ai-rendering.md` (à créer) – pipeline de rendu et intégration avec un orchestrateur (Inngest ou autre).
