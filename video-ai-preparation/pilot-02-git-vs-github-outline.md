---
title: "Pilot 02 – Git vs GitHub (outline complet)"
type: documentation
status: draft
area: video-ai
tags:
  - video-ai
  - pilot
  - thp
  - git
  - github
  - serie-01
created: 2026-03-18
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

# Pilot 02 – Git vs GitHub

Outline complet pour la vidéo « Git vs GitHub » de la [Série 01](serie-01-git-github.md). Format 1 – Concept intro. Une idée par clip : **Git et GitHub ne sont pas la même chose** — Git = outil local (historique / machine à voyager dans le temps), GitHub = hébergement en ligne + lieu de collaboration.

**Agents Cursor** : pour toute itération **script / scènes / Remotion**, exécuter `bun run bootstrap:agents` si besoin, puis charger **thp-video-generation**, **thp-solarpunk-visual** et **remotion-best-practices**. Phrase type et rules : [runbook §08 — Triggers agent](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo). **Avant une itération sur le script** : [Template sync](video-ai-preparation.md#template-sync-before-script-edits).

---

## Implémentation (code)

- **Contenu** : `apps/remotion/src/remotion/compositions/serie-01/pilot02-content.ts` — CPS, pauses et `startFrame` par bloc (source de vérité des timings).
- **Composition** : `Pilot02GitVsGithub.tsx` — id Remotion `Pilot02GitVsGithub`, **1350 frames** (45 s @ 30 fps), 1920×1080, enregistrée dans `Root.tsx`.
- **Taxonomie texte THP** : implémentation selon la source unique `thp-video-generation` + `library-matrix` : **`GlitchText`** (hero intro), **`TextReveal`** (sous-titre intro, titres concept/calmes, CTA), **`Typewriter`** (narration/callout), `WordByWord` optionnel pour une emphase ciblée, comme [`TextDemo.tsx`](../../../apps/remotion/src/remotion/compositions/demos/TextDemo.tsx). **Pas** de simple fade sur `TitleCard` / `ConceptSlide` statiques seuls.
- **V1 clean (série 01)** : `SceneHeader` (6 scènes, mots-clés `TITRE` → `SUITE`), `ProgressBar` footer, shell **`Serie01SceneShell`** (`FadeSlide` catalogue + header ; variante `layout="stack"` pour la scène GitHub + `FlowChart`), mini **`FlowChart`** 2 nœuds en fin de scène 4. Constantes Flow dans `pilot02-content.ts` (`GITHUB_FLOW_*`). `GlitchText` activé sur le hero intro selon règle globale.

---

## Cartographie taxonomie §04 (par scène)

| # | Contenu écran (résumé) | Rôle §04 | Composant | Notes |
|---|------------------------|----------|-----------|--------|
| 1 | Titre + sous-titre | Hero intro + sous-titre intro | `GlitchText`, `TextReveal` | |
| 2 | Hook | Narration | `Typewriter` | |
| 3 | Git — titre + corps | Impact + narration | `TextReveal`, `Typewriter` | |
| 4 | GitHub — titre + corps + callout | Impact + narration + emphase | `TextReveal`, `Typewriter` ×2 | `FlowChart` = schéma, pas taxonomie « texte ». |
| 5 | Recap | Narration | `Typewriter` | |
| 6 | CTA | Hero + CTA | `TextReveal`, `Typewriter` | |
| *opt.* | Une phrase à surligner mot à mot | Emphase | `WordByWord` | **Au plus une** phrase par scène ; non utilisé en V1 — réserver si le script l’exige. |

### Exceptions matrix (obligatoire)

| Bloc | Écart par rapport à matrix | Raison validée | Date |
|------|----------------------------|----------------|------|
| _(aucun)_ | — | — | — |

---

## Pilot metadata

| Field | Value |
|-------|--------|
| **Title** | Git vs GitHub |
| **Format** | Concept intro (Format 1) |
| **Target duration** | 45 s (30–60 s) |
| **FPS (composition)** | 30 → **1350 frames** pour 45 s |
| **Source** | THP – module Git / première approche (à lier au cours existant si disponible) |
| **Paired video** | Série 01 – vidéo 1 (Pré-requis) en amont ; vidéo 3 (Commit) en aval. Pas de Code demo jumelée pour ce clip. |
| **Public** | Débutants THP ; pas de jargon sauf Git / GitHub / commit (introduits ici ou ailleurs). |
| **Idée unique** | Git = outil sur ta machine (historique des versions). GitHub = site qui héberge tes dépôts et permet de travailler à plusieurs. |

---

## Script

Texte prévu pour la voix (voix off). Rythme : ~2 mots/s + pauses = ~45 s. À enregistrer ou à afficher en sous-titres selon le choix de production.

---

**Accroche (hook)**  
« Git et GitHub, tu as déjà vu ces noms. Mais est-ce que c’est la même chose ? En fait non — et comprendre la différence va t’éviter beaucoup de confusion. »

**Concept (une idée)**  
« Git, c’est un outil qui tourne sur ton ordinateur. Il garde l’historique de ton projet : chaque sauvegarde, chaque modification. C’est un peu une machine à voyager dans le temps pour ton code.  
GitHub, c’est un site sur internet. Il stocke une copie de ton projet en ligne et permet de travailler à plusieurs : partager le code, proposer des changements, les valider. En résumé : Git = sur ta machine, GitHub = en ligne et pour la collaboration. »

**Micro-recap**  
« Retiens : Git, c’est l’outil ; GitHub, c’est l’endroit où on le met en commun. »

---

**Notes rédaction**  
- Pas de détails techniques (pas de "remote", "push", "clone" ici — d’autres clips de la série s’en chargent).  
- Schéma « machine ↔ nuage » : **FlowChart** léger en scène 4 (voir Implémentation).

---

## Scene breakdown

Une ligne = une scène. Durées en secondes ; total visé ≈ 45 s.

| # | Purpose | Est. duration | Component(s) | Contenu à l’écran (résumé) |
|---|---------|---------------|--------------|---------------------------|
| 1 | Titre et contexte | 5 s | TitleCard | Titre : « Git vs GitHub ». Sous-titre optionnel : « Une idée en 45 secondes ». |
| 2 | Accroche (hook) | 8 s | SectionIntro | Texte court : « Git et GitHub : est-ce la même chose ? Non — et ça change tout de comprendre la différence. » |
| 3 | Concept – Git (outil local) | 12 s | ConceptSlide | Titre : « Git = sur ta machine ». Corps : « Un outil sur ton ordinateur. Il garde l’historique de ton projet : chaque sauvegarde. Une machine à voyager dans le temps pour ton code. » Pas de callout obligatoire. |
| 4 | Concept – GitHub (en ligne + collaboration) | 12 s | ConceptSlide | Titre : « GitHub = en ligne + collaboration ». Corps : « Un site sur internet. Il stocke une copie de ton projet et permet de travailler à plusieurs : partager le code, proposer des changements, les valider. » Option : callout « Git = machine · GitHub = lieu en ligne ». |
| 5 | Micro-recap | 6 s | SectionIntro ou ConceptSlide | Une phrase : « Git, c’est l’outil ; GitHub, c’est l’endroit où on le met en commun. » |
| 6 | Fin / CTA optionnel | 2 s | TitleCard ou fond neutre | Ex. « À suivre : Commit, késako » ou logo THP. Optionnel ; peut être fusionné avec la scène 5. |

**Total (check)** : 5 + 8 + 12 + 12 + 6 + 2 = **45 s** ≈ durée cible.

---

## Components needed

D’après le [Component shortlist](video-ai-preparation.md#component-shortlist). Équivalent pédagogique : **TextReveal + Typewriter** (`@repo/ui/remotion`) pour les scènes 1–6 + **FlowChart**, **SceneHeader**, **ProgressBar**, **FadeSlide** (V1).

| Rôle outline | Used in scene(s) | Implémentation Remotion |
|--------------|------------------|-------------------------|
| TitleCard | 1, 6 | `TextReveal` + `Typewriter` (sous-titre / CTA). |
| SectionIntro | 2, 5 | `Typewriter`. |
| ConceptSlide | 3, 4 | `TextReveal` (titre) + `Typewriter` (corps) ; callout = `Typewriter` accent. |
| Schéma | 4 | `FlowChart` 2 nœuds (fin de scène). |
| CodeBlockWithHighlight | — | Non utilisé (Format 1). |
| CodeAlongStep | — | Non utilisé (Format 2). |

**Gaps** : aucun.

---

## Informations complémentaires (production)

| Élément | Détail |
|--------|--------|
| **Ton** | Pédagogique, direct, rassurant (« ça va t’éviter beaucoup de confusion »). |
| **Niveau** | Débutant ; pas de prérequis technique au-delà de « tu codes un peu ». |
| **Visuels** | Intro hero en `GlitchText`, sous-titre en `TextReveal`, narration en `Typewriter`, `FlowChart` scène 4, chrome série harmonisé. |
| **Voix** | Une voix ; débit modéré pour laisser le temps de lire si sous-titres. |
| **Musique / fond** | Selon charte THP ; optionnel pour un concept intro court. |

---

## Ready for Remotion when

- [x] Script et scene breakdown remplis et relus (ce document).
- [x] Liste des composants conforme au P0 shortlist ; écarts documentés (aucun ici).
- [x] Durée cible et format conformes aux [Formats](video-ai-preparation.md#video-formats).
- [x] **V1 série (consolidée)** : `Serie01SceneShell` partagé ; hero intro `GlitchText` + sous-titre intro `TextReveal` ; `layout="stack"` scène GitHub + `FlowChart` ; lint + bundle OK.
- [ ] [Checklist visuelle THP Solarpunk](../Templates/thp-solarpunk-visual-checklist.md) passée après rendu Studio.
- [ ] **Agent** : `bun run bootstrap:agents` si besoin ; skills **thp-video-generation**, **thp-solarpunk-visual** et **remotion-best-practices** ; session avec la **phrase type** [§08](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo).

**Suite** : composition `Pilot02GitVsGithub` dans `apps/remotion/.../serie-01/` + `pilot02-content.ts` ; pas de nouveau composant `packages/remotion-lib` requis (wrappers animés P0 déjà présents).
