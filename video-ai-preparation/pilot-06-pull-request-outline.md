---
title: "Pilot 06 – Pull request : proposer et faire valider (outline complet)"
type: documentation
status: draft
area: video-ai
tags:
  - video-ai
  - pilot
  - thp
  - git
  - github
  - pull-request
  - serie-01
created: 2026-03-28
updated: 2026-03-28
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
  - "[[Templates/pilot-outline]]"
  - "[[reference/trigger-prepare-render-fields-inventory]]"
---

# Pilot 06 – Pull request : proposer et faire valider

Outline complet pour la vidéo « Pull request » de la [Série 01](serie-01-git-github.md). Format 1 – Concept intro. **Une idée par clip** : une **pull request** sur GitHub (ou équivalent), c’est **proposer** tes changements pour qu’**un autre** les **relise et valide** avant qu’ils rejoignent la branche principale du dépôt **distant** — à ne pas confondre avec le **merge local** (vidéo 5).

**Agents Cursor** : **thp-video-generation**, **thp-solarpunk-visual**, **remotion-best-practices** — [§08](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo). **Template sync** : [video-ai-preparation](video-ai-preparation.md#template-sync-before-script-edits).

**Storybook / remotion-lib** : non requis — réutilisation `FlowChart`, `Serie01SceneShell`, taxonomie §04.

**Diagrammes §03b 3bis** : non requis — `FlowChart` Remotion.

---

## Format 1 vs graphe

**Format 1** — sections **Graphe conceptuel**, **Storyboard de révélation**, **hero / secondaire** obligatoires ci-dessous.

---

## Pilot metadata

| Field | Value |
|-------|--------|
| **Title** | Pull request : proposer et faire valider |
| **Format** | Concept intro (Format 1) |
| **Target duration** | 45 s (30–60 s) |
| **FPS (composition)** | 30 → **1350 frames** |
| **Source** | THP – module Git / GitHub |
| **Paired video** | Vidéo 5 (Merge) en amont (local) ; vidéo 7 (Fork) en aval. |
| **Public** | Débutants THP ; prérequis : merge conceptuel, usage GitHub évoqué en série. |
| **Idée unique** | La PR est le canal pour **demander** l’intégration + **review** avant fusion sur le dépôt distant. |

---

## Message visuel dominant et hero object (par scène)

| # | Message visuel dominant | Hero (principal) | Secondaire (support) |
|---|-------------------------|------------------|----------------------|
| 1 | Annoncer « Pull request » comme étape sociale / GitHub | `GlitchText` | `TextReveal` sous-titre |
| 2 | Situer : sur la plateforme, on propose avant de fusionner « officiellement » | `Typewriter` hook | Fond |
| 3 | Idée : discussion + validation avant intégration | `TextReveal` titre | `Typewriter` corps |
| 4 | **Chaîne branche → PR → intégration après review** | **`FlowChart`** | Texte court contextuel |
| 5 | Formule à retenir | `Typewriter` recap | — |
| 6 | Enchaînement Fork | `TextReveal` + `Typewriter` CTA | — |

---

## Graphe conceptuel (avant Remotion)

**Nœuds**

- **Branche** — ton travail poussé (commits).
- **Pull request** — demande d’intégration + espace de review.
- **Intégré** — après validation (merge sur la branche cible du remote).

**Arêtes**

| Source | Cible | Relation |
|--------|-------|----------|
| Branche | Pull request | ouvre une |
| Pull request | Intégré | mène à (après review / approbation) |

---

## Storyboard de révélation du schéma (scène 4, frames locales @ 30 fps)

Scène 4 : **360 frames** (12 s). Constantes : `PR_FLOW_FROM_LOCAL`, `PR_FLOWCHART_START_FRAME`, `PR_FLOW_NODE_DELAY` dans `pilot06-content.ts`.

| Beat | Début (local) | Fin (local) | Élément révélé | Lien VO | Entrée / hold / sortie |
|------|---------------|-------------|----------------|---------|------------------------|
| Titre | 0 | ~28 | `TextReveal` « Proposer, puis valider » | Ouverture concept PR | reveal → hold |
| Corps | ~28 | ~200 | `Typewriter` | Phrase sur reviewer / fusion | type → hold |
| Schéma | ~110 | ~126 | `FadeIn` + `FlowChart` | Idée « chaîne » posée | fade-in |
| Nœuds | ~114 | ~350 | Ressorts FlowChart | Parallèle fin d’explication | stagger |
| Hold | ~320 | 360 | Lisible avant cut | Respiration | hold |

---

## Polish visuel (checklist courte)

- [x] Stagger `FlowChart` (`nodeDelay`).
- [x] Schéma tôt (`PR_FLOW_FROM_LOCAL` ~110).
- [x] `FadeIn` sur le bloc diagramme.
- [ ] Studio + [checklist Solarpunk](../Templates/thp-solarpunk-visual-checklist.md) (§ Schémas).

---

## Implémentation (code)

- `pilot06-content.ts`, `Pilot06PullRequest.tsx`, id Remotion **`Pilot06PullRequest`**, **1350 f**, `Root.tsx`, export `serie-01/index.ts`.
- Seed `serie-01-pull-request` + `sceneRegistry`.
- **Trigger** : `compositionId` = `Pilot06PullRequest` — [trigger-prepare-render-fields-inventory](../reference/trigger-prepare-render-fields-inventory.md).

---

## Cartographie taxonomie §04 (par scène)

| # | Contenu écran | Rôle §04 | Composant |
|---|----------------|----------|-----------|
| 1 | Titre + sous-titre | Hero + sous-titre intro | `GlitchText`, `TextReveal` |
| 2 | Hook | Narration | `Typewriter` |
| 3 | Concept PR | Impact + narration | `TextReveal`, `Typewriter` |
| 4 | Schéma + texte | Impact + narration | `TextReveal`, `Typewriter`, `FlowChart` |
| 5 | Recap | Narration | `Typewriter` |
| 6 | CTA | CTA titre + sous-titre | `TextReveal`, `Typewriter` |

### Exceptions matrix

| Bloc | Écart | Raison | Date |
|------|-------|--------|------|
| _(aucun)_ | — | — | — |

---

## Script

**Accroche**  
« Sur GitHub, tu ne « merges » pas dans le vide : tu ouvres une **pull request** pour **proposer** tes changements et laisser **quelqu’un les relire** avant qu’ils partent dans la branche principale du dépôt. »

**Concept**  
« Une pull request, c’est **l’endroit** où la **discussion** et la **validation** se passent — avant que le code rejoigne la ligne stable **à distance**. »

**Micro-recap**  
« Retiens : pull request = **proposer** et **faire valider** avant d’intégrer. »

**Notes**  
- Ne pas refaire tout le cours merge local ; renvoi implicite vidéo 5.  
- Ne pas détailler CI, conflits avancés, ou politique d’équipe.

---

## Scene breakdown

| # | Purpose | Durée | Composants | Résumé écran |
|---|---------|-------|------------|--------------|
| 1 | Titre | 5 s | Serie01SceneShell | « Pull request » + sous-titre |
| 2 | Hook | 8 s | Serie01SceneShell | Proposer + relire avant fusion |
| 3 | Concept | 12 s | Serie01SceneShell | Discussion + validation |
| 4 | Schéma | 12 s | Serie01SceneShell `stack` | Titre + corps + FlowChart |
| 5 | Recap | 6 s | Serie01SceneShell | Une phrase |
| 6 | CTA | 2 s | Serie01SceneShell | « À suivre : Fork » |

**Total** : 45 s.

---

## Text role mapping

| Text block | Role id | Composant | Notes |
|------------|---------|-----------|--------|
| Titre scène 1 | ROLE_INTRO_HERO | GlitchText | |
| Sous-titre 1 | ROLE_INTRO_SUBTITLE | TextReveal | |
| Hook 2 | ROLE_NARRATION | Typewriter | |
| Titres 3–4 | — | TextReveal | |
| Corps 3–4 | ROLE_NARRATION | Typewriter | |
| Recap 5 | ROLE_NARRATION | Typewriter | |
| CTA 6 | ROLE_CTA_TITLE / SUBTITLE | TextReveal, Typewriter | |

---

## Components needed

| Rôle | Scènes | Implémentation |
|------|--------|----------------|
| Serie01SceneShell | 1–6 | Shell série 01 |
| GlitchText, TextReveal, Typewriter | 1–6 | §04 |
| FlowChart | 4 | 3 nœuds horizontaux |
| FadeIn | 4 | Entrée schéma |
| ParticleField / overlays | fond | Comme pilots 04–05 |

**Gaps** : aucun.

---

## Ready for Remotion when

- [x] Graphe + storyboard + hero/secondaire.
- [x] Scène 4 : FlowChart = héros.
- [x] Script, breakdown, text role mapping, components.
- [ ] Checklist Solarpunk + Studio.
- [ ] Agent skills + phrase §08.
