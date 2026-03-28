---
title: "Pilot 05 – Merge : fusionner une branche (outline complet)"
type: documentation
status: draft
area: video-ai
tags:
  - video-ai
  - pilot
  - thp
  - git
  - merge
  - serie-01
created: 2026-03-27
updated: 2026-03-27
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
  - "[[reference/trigger-prepare-render-fields-inventory]]"
  - "[[Templates/pilot-outline]]"
---

# Pilot 05 – Merge : réunir les lignes

Outline complet pour la vidéo « Merge » de la [Série 01](serie-01-git-github.md). Format 1 – Concept intro. **Une idée par clip** : le **merge** intègre l’historique d’une branche dans une **branche cible** (souvent `main`) pour ramener le travail isolé.

**Agents Cursor** : pour toute itération **script / scènes / Remotion**, charger **thp-video-generation**, **thp-solarpunk-visual** et **remotion-best-practices**. Phrase type : [runbook §08](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo). **Avant une itération sur le script** : [Template sync](video-ai-preparation.md#template-sync-before-script-edits).

**Storybook / remotion-lib (conditionnel)** : **non requis** pour ce pilote — réutilisation des mêmes briques que le pilot 04 (`GlitchText`, `TextReveal`, `Typewriter`, `FlowChart`, `Serie01SceneShell`). Pas de nouveau composant `packages/ui` ni `packages/remotion-lib`.

**Diagrammes §03b 3bis (conditionnel)** : **non requis** — pas de `.mmd` dédié ; le schéma pédagogique est le **FlowChart** Remotion (comme pilot 04).

---

## Implémentation (code)

- **Contenu** : `apps/remotion/src/remotion/compositions/serie-01/pilot05-content.ts` — CPS, pauses et `startFrame` par bloc ; `MERGE_FLOW_FROM_LOCAL` calé pour que le **FlowChart** apparaisse tôt en scène 4 (héros visuel).
- **Composition** : `Pilot05Merge.tsx` — id Remotion `Pilot05Merge`, **1350 frames** (45 s @ 30 fps), 1920×1080, enregistrée dans `Root.tsx`.
- **Taxonomie texte THP** : alignée sur pilot 04 — `GlitchText`, `TextReveal`, `Typewriter`, **`FlowChart`** 3 nœuds (branche cible, travail à intégrer, après merge).
- **Catalogue web** : entrée seed `serie-01-merge` + `sceneRegistry` (`Pilot05Merge`) pour cohérence avec les autres épisodes série 01 exposés au catalogue.

**Orchestration Trigger** : inventaire KM des champs utiles (sans figer le contrat Zod) — [trigger-prepare-render-fields-inventory](../reference/trigger-prepare-render-fields-inventory.md).

---

## Cartographie taxonomie §04 (par scène)

| # | Contenu écran (résumé) | Rôle §04 | Composant | Notes |
|---|------------------------|----------|-----------|--------|
| 1 | Titre + sous-titre | Hero intro + sous-titre intro | `GlitchText`, `TextReveal` | |
| 2 | Hook | Narration | `Typewriter` | |
| 3 | Concept — intégration propre | Impact + narration | `TextReveal`, `Typewriter` | |
| 4 | Concept — schéma merge | Impact + narration | `TextReveal`, `Typewriter` | `FlowChart` 3 nœuds. |
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
| **Title** | Merge : réunir les branches |
| **Format** | Concept intro (Format 1) |
| **Target duration** | 45 s (30–60 s) |
| **FPS (composition)** | 30 → **1350 frames** pour 45 s |
| **Source** | THP – module Git / première approche |
| **Paired video** | Série 01 – vidéo 4 (Branch) en amont ; vidéo 6 (Pull request) en aval. |
| **Public** | Débutants THP ; prérequis : branches (vidéo 4). |
| **Idée unique** | Le merge ramène les commits d’une branche dans une branche cible pour unifier l’historique. |

**Format** : **Format 1** — graphe conceptuel + storyboard de révélation **obligatoires** (voir sections ci-dessous, alignées sur [Templates/pilot-outline.md](../Templates/pilot-outline.md)).

---

## Message visuel dominant et hero object (par scène)

| # | Message visuel dominant | Hero (principal) | Secondaire (support) |
|---|-------------------------|------------------|----------------------|
| 1 | Annoncer la notion « Merge » comme réunion des lignes | `GlitchText` titre | `TextReveal` sous-titre |
| 2 | Situer : tu as une branche, tu veux réintégrer | `Typewriter` hook | Fond / particules |
| 3 | Idée : fusion = ajouter sans effacer | `TextReveal` titre concept | `Typewriter` corps |
| 4 | **Le merge vu comme chaîne main → branche → historique unifié** | **`FlowChart` 3 nœuds** (révélation progressive) | Titres + `Typewriter` court (contexte) |
| 5 | Une phrase à retenir | `Typewriter` recap | — |
| 6 | Enchaînement vers Pull request | `TextReveal` CTA | `Typewriter` sous-CTA |

---

## Graphe conceptuel (avant Remotion)

**Nœuds**

- **main** — branche cible (stable).
- **Branche** — travail isolé à intégrer.
- **Après merge** — historique unifié sur la cible.

**Arêtes** (ordre de lecture pédagogique)

| Source | Cible | Relation |
|--------|-------|----------|
| main | Branche | reçoit le travail de |
| Branche | Après merge | une fois fusionnée, devient |
| (implicite) | main | la cible **contient** l’historique unifié après merge |

---

## Storyboard de révélation du schéma (scène 4, frames locales @ 30 fps)

Scène 4 globale : **360 frames** (12 s). Réf. code : `MERGE_FLOW_FROM_LOCAL`, `MERGE_FLOWCHART_START_FRAME`, `MERGE_FLOW_NODE_DELAY` dans `pilot05-content.ts` (ajustés pour que le **schéma monte avant** la fin du corps — héros visuel).

| Beat | Début (local) | Fin (local) | Élément révélé | Lien VO | Entrée / hold / sortie |
|------|---------------|-------------|----------------|---------|------------------------|
| Titre scène | 0 | ~28 | `TextReveal` « La branche cible grandit » | Première phrase du concept schéma | reveal → hold lecture |
| Corps | ~28 | ~200 | `Typewriter` explication | « Après un merge réussi… » | type → hold |
| Entrée schéma | ~110 | ~126 | `FadeIn` + socle `FlowChart` | Dès que l’idée « unifier » est posée | fade-in ~16 f |
| Nœuds | ~114 | ~350 | Ressorts `FlowChart` (main → Branche → Après merge) | En parallèle / juste après le corps | stagger via `nodeDelay` |
| Hold final | ~320 | 360 | Graphe + texte lisibles | Fin de phrase / respiration avant cut | hold |

---

## Polish visuel (checklist courte)

- [x] **Stagger** : `FlowChart` avec `nodeDelay` entre nœuds.
- [x] **Schéma tôt dans la scène 4** : `MERGE_FLOW_FROM_LOCAL` réduit vs pilot 04 équivalent pour donner plus de temps « héros » au graphe (voir `pilot05-content.ts`).
- [x] **FadeIn** sur le bloc diagramme.
- [ ] **Studio** : vérifier hold final avant cut scène 4 ; checklist Solarpunk complète.

---

## Script

Texte prévu pour la voix (voix off). Rythme : ~2 mots/s + pauses ≈ 45 s.

**Accroche (hook)**  
« Tu as travaillé sur une branche à part. Le **merge**, c’est l’étape où tu **ramènes** ce travail dans la branche cible — souvent *main* — en **intégrant** les commits. »

**Concept (deux temps à l’écran)**  
« Une fusion, c’est **ajouter** l’historique de ta branche à la ligne cible **sans effacer** ce qui y était déjà — quand Git peut le faire **proprement**.  
Après un merge réussi sur *main*, ta ligne stable **contient aussi** les commits venus de la branche fusionnée. »

*(Ne pas détailler conflits, fast-forward ou stratégies avancées dans ce clip.)*

**Micro-recap**  
« Retiens : merge = intégrer une branche dans une autre pour réunir les lignes. »

**Notes rédaction**  
- Ne pas confondre avec **Pull request** (vidéo 6) : ici focus **concept local** de fusion.  
- Schéma scène 4 : **FlowChart** horizontal — **main** (cible) → **Branche** (travail) → **Après merge** (unifié).

---

## Scene breakdown

| # | Purpose | Est. duration | Component(s) | Contenu à l’écran (résumé) |
|---|---------|---------------|--------------|---------------------------|
| 1 | Titre et contexte | 5 s | Serie01SceneShell | « Merge » + « Réunir les lignes ». |
| 2 | Accroche (hook) | 8 s | Serie01SceneShell | Branche → ramener → intégrer dans la cible. |
| 3 | Concept — intégration | 12 s | Serie01SceneShell | Titre + corps : ajouter sans effacer. |
| 4 | Concept — schéma | 12 s | Serie01SceneShell (`layout="stack"`) | Titre + corps + **FlowChart**. |
| 5 | Micro-recap | 6 s | Serie01SceneShell | Une phrase récap. |
| 6 | Fin / CTA | 2 s | Serie01SceneShell | « À suivre : Pull request ». |

**Total (check)** : 5 + 8 + 12 + 12 + 6 + 2 = **45 s** ≈ durée cible.

---

## Text role mapping (required, before code)

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
| FlowChart | 4 | 3 nœuds horizontaux (main cible → Branche → Après merge). |
| FadeIn | 4 | Entrée du schéma. |
| ParticleField / overlays | fond | Aligné pilots 01–04. |

**Gaps** : aucun.

---

## Informations complémentaires (production)

| Élément | Détail |
|--------|--------|
| **Ton** | Tutoiement, direct ([thp-tone-and-theme](../reference/thp-tone-and-theme.md)). |
| **Visuels** | Transitions variées (cohérent pilot 04). |

---

## Ready for Remotion when

- [x] Script et scene breakdown remplis et relus (ce document).
- [x] **Format 1** : graphe conceptuel + storyboard de révélation + table hero/secondaire (sections ci-dessus).
- [x] Scène 4 : **FlowChart** = héros (pas décoratif).
- [x] Text role mapping et composants alignés matrix / shortlist.
- [x] Durée cible et format conformes aux [Formats](video-ai-preparation.md#video-formats).
- [x] Polish visuel : items applicables cochés ou reportés.
- [ ] [Checklist visuelle THP Solarpunk](../Templates/thp-solarpunk-visual-checklist.md) passée après rendu Studio (incl. § **Schémas**).
- [ ] **Agent** : skills **thp-video-generation**, **thp-solarpunk-visual** et **remotion-best-practices** ; phrase type [§08](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo).

**Suite** : composition `Pilot05Merge` + `pilot05-content.ts` ; pas de nouveau composant `packages/remotion-lib` requis.
