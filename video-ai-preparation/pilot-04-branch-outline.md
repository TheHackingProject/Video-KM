---
title: "Pilot 04 – Branch : ligne de travail parallèle (outline complet)"
type: documentation
status: draft
area: video-ai
tags:
  - video-ai
  - pilot
  - thp
  - git
  - branch
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

# Pilot 04 – Branch : une ligne parallèle

Outline complet pour la vidéo « Branch » de la [Série 01](serie-01-git-github.md). Format 1 – Concept intro. **Une idée par clip** : une **branche** = une ligne de développement **parallèle** pour expérimenter sans mélanger avec la référence stable.

**Agents Cursor** : pour toute itération **script / scènes / Remotion**, exécuter `bun run bootstrap:agents` si besoin, puis charger **thp-video-generation**, **thp-solarpunk-visual** et **remotion-best-practices**. Phrase type et rules : [runbook §08 — Triggers agent](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo). **Avant une itération sur le script** : [Template sync](video-ai-preparation.md#template-sync-before-script-edits).

---

## Implémentation (code)

- **Contenu** : `apps/remotion/src/remotion/compositions/serie-01/pilot04-content.ts` — CPS, pauses et `startFrame` par bloc (source de vérité des timings).
- **Composition** : `Pilot04Branch.tsx` — id Remotion `Pilot04Branch`, **1350 frames** (45 s @ 30 fps), 1920×1080, enregistrée dans `Root.tsx`.
- **Taxonomie texte THP** : `thp-video-generation` + `library-matrix` : **`GlitchText`** (hero intro), **`TextReveal`** (sous-titre intro, titres concept, CTA titre), **`Typewriter`** (narration), comme [`Pilot03Commit.tsx`](../../../apps/remotion/src/remotion/compositions/serie-01/Pilot03Commit.tsx).
- **V1 série 01** : `SceneHeader` (6 scènes, mots-clés dédiés), `ProgressBar` footer, shell **`Serie01SceneShell`** (transitions variées), **`FlowChart`** 3 nœuds en scène 4 (**main** → **Branche** → **Commits** / chaîne horizontale).

---

## Cartographie taxonomie §04 (par scène)

| # | Contenu écran (résumé) | Rôle §04 | Composant | Notes |
|---|------------------------|----------|-----------|--------|
| 1 | Titre + sous-titre | Hero intro + sous-titre intro | `GlitchText`, `TextReveal` | |
| 2 | Hook | Narration | `Typewriter` | |
| 3 | Concept — analogie voie de garage | Impact + narration | `TextReveal`, `Typewriter` | |
| 4 | Concept — parallélisme + schéma | Impact + narration | `TextReveal`, `Typewriter` | `FlowChart` 3 nœuds. |
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
| **Title** | Branch : une ligne parallèle |
| **Format** | Concept intro (Format 1) |
| **Target duration** | 45 s (30–60 s) |
| **FPS (composition)** | 30 → **1350 frames** pour 45 s |
| **Source** | THP – module Git / première approche |
| **Paired video** | Série 01 – vidéo 3 (Commit) en amont ; vidéo 5 (Merge) en aval. Pas de Code demo jumelée pour ce clip. |
| **Public** | Débutants THP ; prérequis : commits (vidéo 3). |
| **Idée unique** | Une branche = une ligne de travail parallèle pour isoler des changements avant de les réintégrer. |

---

## Script

Texte prévu pour la voix (voix off). Rythme : ~2 mots/s + pauses ≈ 45 s.

---

**Accroche (hook)**  
« Tu sais enregistrer des commits. Parfois tu veux avancer une idée **sans mélanger** ça avec ta version stable : Git te permet d’ouvrir une **branche** — une ligne de développement **parallèle**. »

**Concept (deux temps à l’écran)**  
« Une branche, c’est comme une **voie de garage** : la route principale continue, et toi tu travailles à part.  
Sur une branche, tes commits restent **à côté** de la ligne principale : tu n’écrases pas *main* tant que tu n’as pas **fusionné**. »

*(GitFlow : au plus une phrase en VO si la durée le permet — sinon renvoi à une série avancée ; ne pas détailler `git branch` / `checkout` dans ce clip.)*

**Micro-recap**  
« Retiens : une branche = une ligne de travail parallèle. »

---

**Notes rédaction**  
- Ne pas entrer dans `merge` en détail (vidéo 5).  
- Schéma scène 4 : **FlowChart** horizontal — **main** → **Branche** → **Commits** (libellés courts lisibles à l’écran).

---

## Scene breakdown

| # | Purpose | Est. duration | Component(s) | Contenu à l’écran (résumé) |
|---|---------|---------------|--------------|---------------------------|
| 1 | Titre et contexte | 5 s | Serie01SceneShell | « Branch » + « Une ligne parallèle ». |
| 2 | Accroche (hook) | 8 s | Serie01SceneShell | Commits → besoin d’isoler → la branche. |
| 3 | Concept — analogie | 12 s | Serie01SceneShell | Titre + corps : voie de garage / route principale. |
| 4 | Concept — parallélisme + schéma | 12 s | Serie01SceneShell (`layout="stack"`) | Titre + corps + **FlowChart** main → Branche → Commits. |
| 5 | Micro-recap | 6 s | Serie01SceneShell | Une phrase récap. |
| 6 | Fin / CTA | 2 s | Serie01SceneShell | « À suivre : Merge ». |

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
| FlowChart | 4 | 3 nœuds horizontaux (main → Branche → Commits). |
| FadeIn | 4 | Entrée du schéma. |
| ParticleField / overlays | fond | Aligné pilots 01–03 (faible opacité). |

**Gaps** : aucun.

---

## Informations complémentaires (production)

| Élément | Détail |
|--------|--------|
| **Ton** | Tutoiement, direct ([thp-tone-and-theme](../reference/thp-tone-and-theme.md)). |
| **Visuels** | Transitions **variées** vs pilot 03 (ex. zoom-blur titre, wipe hook, etc.). |

---

## Ready for Remotion when

- [x] Script et scene breakdown remplis et relus (ce document).
- [x] Text role mapping et composants alignés matrix / shortlist.
- [x] Durée cible et format conformes aux [Formats](video-ai-preparation.md#video-formats).
- [ ] [Checklist visuelle THP Solarpunk](../Templates/thp-solarpunk-visual-checklist.md) passée après rendu Studio.
- [ ] **Agent** : skills **thp-video-generation**, **thp-solarpunk-visual** et **remotion-best-practices** ; phrase type [§08](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo).

**Suite** : composition `Pilot04Branch` + `pilot04-content.ts` ; pas de nouveau composant `packages/remotion-lib` requis.
