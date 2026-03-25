---
title: "Soul — recherche visuelle & vulgarisation pour les vidéos THP"
type: research
diataxis: explanation
status: published
area: video-ai
tags:
  - research
  - video-ai
  - vulgarisation
  - visuals
  - workflow
created: 2026-03-25
updated: 2026-03-25
related:
  - "[[research/README]]"
  - "[[video-ai-preparation/video-ai-preparation]]"
  - "[[runbooks/video-ai-development]]"
  - "[[reference/solarpunk-theme-decisions]]"
---

# Soul — recherche visuelle & vulgarisation

**Soul** (ou **esprit** du sujet) désigne ici : **un fichier de recherche vivant par thème ou par vidéo**, alimenté par de **petites passes de recherche** (culture pop, métaphores, pédagogie cognitive, ce qui “lit” bien à l’écran). Ce n’est **pas** la spec canonique du produit : c’est une **couche créative** qui nourrit l’outline, le découpage de scènes et le choix des blocs Remotion **avant** et **pendant** l’écriture du script détaillé.

## Objectif

- Réduire la dépendance au **mur de texte** seul.
- Viser des **images mentales partagées** (grand public, débutants THP).
- Relier chaque **idée** à au moins un **artefact visuel** compréhensible sans lire tous les sous-titres (schéma, prop, split, mouvement de caméra léger, comparaison côte à côte, etc.).

## Où ça vit dans le repo

| Élément | Rôle |
|--------|------|
| **`KM/Docs/research/<sujet>-vulgarisation-visuelle.md`** | Fichier Soul **par sujet** (ex. Git/GitHub). Chaque mini-recherche y ajoute une sous-section datée ou un bloc “à intégrer”. |
| **`KM/Docs/video-ai-preparation/pilot-*-outline.md`** | Colonne ou section **« Cues visuels / Soul »** par scène : 1–3 bullets issus du fichier research (pas le roman complet). |
| **Skill `thp-video-generation`** | Rappelle d’**ouvrir** le fichier Soul du sujet **avant** de figer les composants par scène. |
| **`thp-solarpunk-visual`** | Traduit les cues en **tokens**, contraste, motion **sans** casser la lisibilité. |

## Workflow recommandé (agent ou humain)

1. **Créer ou ouvrir** le fichier `research/<slug>-vulgarisation-visuelle.md` pour le thème de la vidéo (ex. série 01 Git).
2. **Mini-recherche** (15–30 min) : 2–3 sources max + intuition pédagogique → ajouter une section **« Incrément — YYYY-MM-DD »** avec : métaphore testée, risque de confusion, **ce qu’on montre** (écran), **ce qu’on évite**.
3. **Traduire en outline** : pour chaque scène du pilot, remplir **1 idée visuelle forte** + composant cible (`FlowChart`, `Terminal`, `ComparisonTable`, illustration SVG, etc.).
4. **Storybook / démo** si nouveau bloc réutilisable ; sinon réutiliser le **catalogue démos** existant.
5. **Revue** : “un spectateur sans son” comprend-il l’idée en 5 s ? (test rapide sur une still Remotion.)

## Critères “grand public”

- **Une métaphore = une image** (pas trois métaphores en 20 s).
- **Jargon** : apparaît **après** l’image ou le geste, pas avant.
- **Densité** : alterner **texte court** + **non-texte** (diagramme, terminal, picto, silence visuel).
- **Cohérence série** : réutiliser les **mêmes props visuels** d’un pilote à l’autre (ex. “ligne du temps” pour l’historique Git).

## Fichiers Soul existants (index)

- [git-github-vulgarisation-visuelle](git-github-vulgarisation-visuelle.md) — série 01, concepts Git / GitHub / workflow.

*(Ajouter ici une ligne par nouveau sujet.)*

## Promotion hors research

Quand une métaphore ou un pattern visuel devient **standard de la série**, documenter la décision dans [solarpunk-theme-decisions](../reference/solarpunk-theme-decisions.md) ou dans l’outline de série, pas seulement dans ce dossier.
