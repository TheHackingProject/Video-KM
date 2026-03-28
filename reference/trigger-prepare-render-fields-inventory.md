---
title: "Inventaire KM — champs utiles prepare / render Trigger"
type: reference
status: draft
area: video-ai
tags:
  - trigger
  - orchestration
  - contracts
created: 2026-03-27
updated: 2026-03-27
related:
  - "[[runbooks/video-ai-rendering]]"
  - "[[runbooks/api]]"
  - "[[research/trigger-dev]]"
---

# Inventaire KM — champs utiles pour préparer / rendre (Trigger)

**But** : liste de référence pour ne rien oublier quand `@repo/contracts` et les tasks `apps/trigger` évoluent. **Ce document ne fige pas** le schéma Zod actuel ; il complète le payload minimal existant.

**Code aujourd’hui** : `RenderPipelinePayloadSchema` (`compositionId`, `correlationId` optionnel) ; chaîne `prepare-render` → `render-video` (stub) → `notify-render` — voir [`apps/trigger/src/trigger/renderPipeline.ts`](../../../apps/trigger/src/trigger/renderPipeline.ts).

---

## Chaîne de tasks (référence)

| Ordre | Task id | Rôle | Entrée (conceptuelle) | Sortie (conceptuelle) |
|-------|---------|------|----------------------|------------------------|
| 1 | `prepare-render` | Valider le job, résoudre bundle / métadonnées rendu | Payload pipeline | `compositionId`, `bundleHint`, `correlationId` |
| 2 | `render-video` | Exécuter le rendu (Chrome + FFmpeg en prod cible) | Sortie prepare | Résultat fichier / URL / erreur |
| 3 | `notify-render` | Propager fin de pipeline (webhook, DB, log) | compositionId + résultat render | `{ ok: true }` |

---

## Champs utiles à considérer (futur contrat ou env)

| Champ / donnée | Où | Pourquoi |
|----------------|-----|----------|
| `compositionId` | Payload API → Trigger | Id Remotion (`Pilot05Merge`, etc.) — **déjà requis** |
| `correlationId` | Payload | Traçabilité bout-en-bout, logs — **déjà optionnel** |
| `videoId` ou `slug` catalogue | Payload ou DB lookup | Lier le rendu à `videos` / `video_versions` (seed, catalogue THP) |
| `outputPath` / `renderUrl` cible | prepare ou render | Où écrire le fichier ou enregistrer l’URL après upload |
| `qualityProfile` / résolution / codec | Payload ou config worker | Parité avec runbook rendering (1080p, bitrate) |
| `gitRef` / branche / commit | Payload (optionnel) | Reproducibilité : quel SHA du repo pour le bundle Remotion |
| `idempotencyKey` | Headers ou payload | Éviter doubles rendus pour le même épisode + version |
| Secrets (S3, registry, Trigger token) | Env worker / Trigger | Jamais dans le payload public ; runbooks infra |

---

## Lien workflow auteur (vidéo 05)

Quand une nouvelle composition est ajoutée (ex. **Merge**), l’outline KM doit mentionner :

- **Id Remotion** enregistré dans `Root.tsx` (valeur exacte de `compositionId` pour les jobs).
- Si l’épisode est au **catalogue** : `slug`, `compositionId` dans seed + `sceneRegistry` — pour que l’API et le frontend restent alignés avec Trigger.

Pilot de référence : [pilot-05-merge-outline](../video-ai-preparation/pilot-05-merge-outline.md).

---

## Mise à jour

Quand un champ est ajouté à `RenderPipelinePayloadSchema`, **mettre à jour** ce fichier pour indiquer « **implémenté** » et pointer vers le commit ou l’ADR si besoin.
