---
title: "Série 01 – Git & GitHub (base de travail)"
type: documentation
status: draft
area: video-ai
tags:
  - video-ai
  - preparation
  - serie
  - git
  - github
  - thp
created: 2026-03-18
updated: 2026-03-21
related:
  - "[[video-ai-preparation/video-ai-preparation]]"
  - "[[reference/video-lifecycle]]"
---

# Série 01 – Git & GitHub

Base de travail pour la première série de vidéos. **Une idée = un clip** (Format 1 : 30–60 s). Chaque ligne devient un pilote (outline dédié) quand on rédige le script et le découpage. Référence : [Formats](video-ai-preparation.md#video-formats).

**Chrome partagé (pilots 01 & 02)** : taxonomie texte [§04 runbook](../runbooks/video-ai-development.md#taxonomie-texte-thp-reproductible), `solarTheme`, `SceneHeader` + `ProgressBar`, entrées **`FadeSlide`** (catalogue), `ParticleField` à densité harmonisée entre les compositions ; shell **`Serie01SceneShell`** pour factoriser header + slide (voir `apps/remotion/.../serie-01/Serie01SceneShell.tsx`).

---

## Logique pédagogique et gestion du temps

- **Ordre** : pré-requis (pour pouvoir suivre) → Git vs GitHub (clarifier où ça se passe) → Commit (unité de base) → Branch (parallélisme) → Merge (réunir) → Pull request + review (workflow typique) → Fork (copie d’un repo). Optionnel : git diff (voir les changements) ; GitFlow peut rester une phrase dans “Branch” ou un clip dédié plus tard.
- **Durée** : Format 1 = 30–60 s par concept. Éviter de mettre plusieurs notions dans un seul clip (ex. “PR + diff + reviewer + Merge + Fork” = 5 idées → 5 clips possibles, ou regrouper uniquement ce qui forme une seule idée, ex. “PR = proposer + faire valider”).
- **Découpage** : un titre = une idée claire, scriptable en 2–4 phrases + micro-recap.

---

## Ordre de la série (remodélé)

| # | Une idée par clip | Format | Durée cible | Outline |
|---|-------------------|--------|-------------|---------|
| 1 | **Pré-requis** : terminal & bases (pour suivre les démos) | 2 – Code demo guided | 2–5 min | [pilot-01-prerequis-outline](pilot-01-prerequis-outline.md) |
| 2 | **Git vs GitHub** : Git = outil local (machine à voyager dans le temps) · GitHub = hébergement + collaboration | 1 – Concept intro | 30–60 s | [pilot-02-git-vs-github-outline](pilot-02-git-vs-github-outline.md) |
| 3 | **Commit** : une sauvegarde datée du projet | 1 – Concept intro | 30–60 s | *à créer* |
| 4 | **Branch** : une ligne de travail parallèle (et en une phrase : GitFlow = une façon d’utiliser les branches, ou clip dédié plus tard) | 1 – Concept intro | 30–60 s | *à créer* |
| 5 | **Merge** : fusionner une branche dans une autre | 1 – Concept intro | 30–60 s | *à créer* |
| 6 | **Pull request (et review)** : proposer ses changements et les faire valider par quelqu’un | 1 – Concept intro | 30–60 s | *à créer* |
| 7 | **Fork** : copier un repo sous son compte (pour contribuer ou partir de chez soi) | 1 – Concept intro | 30–60 s | *à créer* |
| 8 | **git diff** (optionnel) : voir ce qui a changé entre deux états | 1 – Concept intro | 30–60 s | *à créer* |

---

## Résumé par bloc

- **Bloc 1 – Pré-requis** : vidéo 1 (Format 2) — nécessaire pour la suite.
- **Bloc 2 – Distinction** : vidéo 2 — Git ≠ GitHub.
- **Bloc 3 – Bases locales** : vidéos 3–4 — Commit, Branch.
- **Bloc 4 – Intégration & collaboration** : vidéos 5–7 — Merge, Pull request (+ review), Fork.
- **Bloc 5 – Optionnel** : vidéo 8 — git diff (utile avant ou après “Pull request” selon le script).

GitFlow : soit une phrase dans la vidéo “Branch”, soit un clip séparé en série avancée (éviter de surcharger le premier pilote).

---

## Prochaine étape

Pour chaque vidéo : copier [Templates/pilot-outline.md](../Templates/pilot-outline.md) dans ce dossier (ex. `pilot-01-prerequis-outline.md`, `pilot-02-git-vs-github-outline.md`, …), remplir métadonnées (dont **vidéo jumelle** si besoin), script, scene breakdown, **Components needed** et checklist **Ready for Remotion** du template, puis implémentation dans `apps/remotion` + `packages/remotion-lib`. **À chaque itération sur le script** : resynchroniser avec le template si besoin — [video-ai-preparation § Template sync](video-ai-preparation.md#template-sync-before-script-edits).
