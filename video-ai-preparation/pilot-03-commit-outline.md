---
title: "Pilot 03 – Commit : sauvegarde datée (outline complet)"
type: documentation
status: draft
area: video-ai
tags:
  - video-ai
  - pilot
  - thp
  - git
  - commit
  - serie-01
created: 2026-03-21
updated: 2026-03-21
related:
  - "[[video-ai-preparation/video-ai-preparation]]"
  - "[[video-ai-preparation/serie-01-git-github]]"
  - "[[reference/video-lifecycle]]"
  - "[[reference/thp-tone-and-theme]]"
  - "[[reference/solarpunk-theme-decisions]]"
  - "[[Templates/thp-solarpunk-visual-checklist]]"
  - "[[meta/thp-video-generation-skill]]"
  - "[[meta/thp-solarpunk-visual-skill]]"
  - "[[runbooks/video-ai-development]]"
---

# Pilot 03 – Commit : une sauvegarde datée

Outline complet pour la vidéo « Commit » de la [Série 01](serie-01-git-github.md). Format 1 – Concept intro. **Une idée par clip** : un **commit** = une sauvegarde datée de ton projet à un instant T.

**Agents Cursor** : pour toute itération **script / scènes / Remotion**, exécuter `bun run bootstrap:agents` si besoin, puis charger **thp-video-generation**, **thp-solarpunk-visual** et **remotion-best-practices**. Phrase type et rules : [runbook §08 — Triggers agent](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo). **Avant une itération sur le script** : [Template sync](video-ai-preparation.md#template-sync-before-script-edits).

---

## Implémentation (code)

- **Contenu** : `apps/remotion/src/remotion/compositions/serie-01/pilot03-content.ts` — CPS, pauses et `startFrame` par bloc (source de vérité des timings).
- **Composition** : `Pilot03Commit.tsx` — id Remotion `Pilot03Commit`, **1350 frames** (45 s @ 30 fps), 1920×1080, enregistrée dans `Root.tsx`.
- **Taxonomie texte THP** : `thp-video-generation` + `library-matrix` : **`GlitchText`** (hero intro), **`TextReveal`** (sous-titre intro, titres concept, CTA titre), **`Typewriter`** (narration), comme [`TextDemo.tsx`](../../../apps/remotion/src/remotion/compositions/demos/TextDemo.tsx).
- **V1 série 01** : `SceneHeader` (6 scènes, mots-clés `TITRE`, `ACCROCHE`, `CONCEPT`, `WORKFLOW`, `RÉCAP`, `SUITE`), `ProgressBar` footer, shell **`Serie01SceneShell`** (transitions variées vs pilot 02), **`FlowChart`** 3 nœuds en fin de scène 4 (Modifier → Enregistrer → Commit).

---

## Cartographie taxonomie §04 (par scène)

| # | Contenu écran (résumé) | Rôle §04 | Composant | Notes |
|---|------------------------|----------|-----------|--------|
| 1 | Titre + sous-titre | Hero intro + sous-titre intro | `GlitchText`, `TextReveal` | |
| 2 | Hook | Narration | `Typewriter` | |
| 3 | Concept — sauvegarde / analogie | Impact + narration | `TextReveal`, `Typewriter` | |
| 4 | Concept — workflow + schéma | Impact + narration | `TextReveal`, `Typewriter` | `FlowChart` = schéma 3 nœuds. |
| 5 | Recap | Narration | `Typewriter` | |
| 6 | CTA | CTA titre + sous-titre | `TextReveal`, `Typewriter` | |

### Exceptions matrix (obligatoire)

| Bloc | Écart par rapport à matrix | Raison validée | Date |
|------|----------------------------|----------------|------|
| _(aucun)_ | — | — | — |

---

## Pilot metadata

| Field | Value |
|-------|--------|
| **Title** | Commit : une sauvegarde datée |
| **Format** | Concept intro (Format 1) |
| **Target duration** | 45 s (30–60 s) |
| **FPS (composition)** | 30 → **1350 frames** pour 45 s |
| **Source** | THP – module Git / première approche |
| **Paired video** | Série 01 – vidéo 2 (Git vs GitHub) en amont ; vidéo 4 (Branch) en aval. Pas de Code demo jumelée pour ce clip. |
| **Public** | Débutants THP ; prérequis : notions Git locales (vidéo 2). |
| **Idée unique** | Un commit = une sauvegarde datée de ton projet à un instant T. |

---

## Script

Texte prévu pour la voix (voix off). Rythme : ~2 mots/s + pauses ≈ 45 s.

---

**Accroche (hook)**  
« Tu as compris que Git garde l’historique de ton projet. Mais comment ? Chaque fois que tu enregistres une étape, Git crée un **commit**. »

**Concept (une idée, deux temps à l’écran)**  
« Un commit, c’est une sauvegarde datée : une photo de ton projet à un instant précis. Comme un point de sauvegarde dans un jeu vidéo.  
Tu modifies des fichiers, tu demandes à Git d’enregistrer, puis tu valides : un nouveau commit ajoute une marque dans ton historique. »

**Micro-recap**  
« Retiens : un commit = une sauvegarde datée de ton projet. »

---

**Notes rédaction**  
- Ne pas entrer dans `git add` / `git commit` en détail (clip suivant ou démo dédiée).  
- Schéma workflow : **FlowChart** 3 nœuds en scène 4.

---

## Scene breakdown

| # | Purpose | Est. duration | Component(s) | Contenu à l’écran (résumé) |
|---|---------|---------------|--------------|---------------------------|
| 1 | Titre et contexte | 5 s | Serie01SceneShell | « Commit » + « Une sauvegarde datée » (sous-titre court). |
| 2 | Accroche (hook) | 8 s | Serie01SceneShell | Historique Git → comment ? → le commit. |
| 3 | Concept — analogie | 12 s | Serie01SceneShell | Titre + corps : photo instantanée / jeu vidéo. |
| 4 | Concept — workflow | 12 s | Serie01SceneShell | Titre + corps + **FlowChart** Modifier → Enregistrer → Commit. |
| 5 | Micro-recap | 6 s | Serie01SceneShell | Une phrase récap. |
| 6 | Fin / CTA | 2 s | Serie01SceneShell | « À suivre : Branch ». |

**Total (check)** : 5 + 8 + 12 + 12 + 6 + 2 = **45 s** ≈ durée cible.

---

## Text role mapping (required, before code)

Source : `packages/skills/thp-video-generation/references/library-matrix.md`

| Text block | Role id | Target component | Timing/color notes | Exception reason (if any) |
|------------|---------|------------------|--------------------|---------------------------|
| Titre scène 1 | ROLE_INTRO_HERO | GlitchText | glitch duration + pause | — |
| Sous-titre scène 1 | ROLE_INTRO_SUBTITLE | TextReveal | après hero | — |
| Hook scène 2 | ROLE_NARRATION | Typewriter | CPS dans content | — |
| Titres scènes 3–4 | — | TextReveal | reveal duration | calm concept title |
| Corps scènes 3–4 | ROLE_NARRATION | Typewriter | CPS + startFrame | — |
| Recap scène 5 | ROLE_NARRATION | Typewriter | CPS | — |
| CTA titre scène 6 | ROLE_CTA_TITLE | TextReveal | court | — |
| CTA sous-titre scène 6 | ROLE_CTA_SUBTITLE | Typewriter | court | — |

---

## Components needed

| Rôle outline | Used in scene(s) | Implémentation Remotion |
|--------------|------------------|-------------------------|
| Serie01SceneShell | 1–6 | Shell série 01 + transitions. |
| SceneHeader / ProgressBar | global | Chrome partagé série 01. |
| GlitchText, TextReveal, Typewriter | 1–6 | Taxonomie §04. |
| FlowChart | 4 | 3 nœuds horizontaux. |
| ParticleField / overlays | fond | Aligné pilots 01/02 (faible opacité). |

**Gaps** : aucun.

---

## Informations complémentaires (production)

| Élément | Détail |
|--------|--------|
| **Ton** | Tutoiement, direct ([thp-tone-and-theme](../reference/thp-tone-and-theme.md)). |
| **Visuels** | Transitions **différentes** du pilot 02 (ex. wipe sur titre, zoom-blur sur hook, etc.). |

---

## Ready for Remotion when

- [x] Script et scene breakdown remplis et relus (ce document).
- [x] Text role mapping et composants alignés matrix / shortlist.
- [x] Durée cible et format conformes aux [Formats](video-ai-preparation.md#video-formats).
- [ ] [Checklist visuelle THP Solarpunk](../Templates/thp-solarpunk-visual-checklist.md) passée après rendu Studio.
- [ ] **Agent** : skills **thp-video-generation**, **thp-solarpunk-visual**, **remotion-best-practices** ; phrase type [§08](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo).

**Suite** : composition `Pilot03Commit` + `pilot03-content.ts` ; pas de nouveau composant `packages/remotion-lib` requis.
