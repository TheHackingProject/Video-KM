---
title: "Pilot 01 – Pré-requis : terminal et bases (outline complet)"
type: documentation
status: draft
area: video-ai
tags:
  - video-ai
  - pilot
  - thp
  - terminal
  - serie-01
created: 2026-03-18
updated: 2026-03-18
related:
  - "[[video-ai-preparation/video-ai-preparation]]"
  - "[[video-ai-preparation/serie-01-git-github]]"
  - "[[reference/video-lifecycle]]"
---

# Pilot 01 – Pré-requis : terminal et bases

Outline complet pour la vidéo « Pré-requis » de la [Série 01](serie-01-git-github.md). Format 2 – Code demo guided. **Idée unique** : Savoir ouvrir le terminal et exécuter une ou deux commandes de base pour pouvoir suivre les démos Git de la série.

---

## Pilot metadata

| Field | Value |
|-------|--------|
| **Title** | Pré-requis : terminal et bases |
| **Format** | Code demo guided (Format 2) |
| **Target duration** | 3 min (180 s), fourchette 2–5 min |
| **Source** | THP – pré-requis module Git |
| **Paired video** | Série 01 – vidéo 2 (Git vs GitHub) en aval. |
| **Public** | Débutants THP ; pas de prérequis technique. |
| **Idée unique** | Savoir ouvrir le terminal et exécuter une ou deux commandes de base pour pouvoir suivre les démos de la série. |

---

## Script

Structure Format 2 : intro → objectif → étapes (1–2 phrases + ce qui s’affiche) → recap. Rythme adapté à ~3 min.

---

**Intro (courte)**  
« Dans les prochaines vidéos on va utiliser Git en ligne de commande. Pour suivre, il te faut juste le terminal et deux commandes de base. »

**Objectif**  
« On va voir où ouvrir le terminal sur ton ordinateur, puis une ou deux commandes indispensables. »

**Step 1 – Ouvrir le terminal**  
« Le terminal, c’est une fenêtre où tu tapes des commandes. Sur Mac : cherche “Terminal” dans le Spotlight ou dans Applications > Utilitaires. Sur Linux : souvent Ctrl+Alt+T ou cherche “Terminal” dans le menu. Sur Windows : on peut utiliser PowerShell ou l’invite de commandes ; pour une expérience proche de Mac/Linux, installe WSL ou Git Bash. »  
*À l’écran* : raccourci ou chemin selon OS (à illustrer par une capture ou un schéma simple).

**Step 2 – Première commande : où je suis ?**  
« Une fois le terminal ouvert, tape `pwd` puis Entrée. P-W-D veut dire “print working directory” : ça affiche le dossier dans lequel tu te trouves. »  
*À l’écran* : terminal avec la commande `pwd` et un exemple de sortie (ex. `/Users/toto/projets`).

**Step 3 – Deuxième commande : lister les fichiers**  
« Pour voir le contenu du dossier actuel, tape `ls` puis Entrée. Sur Windows PowerShell tu peux utiliser `dir`. Tu obtiens la liste des fichiers et dossiers. »  
*À l’écran* : terminal avec `ls` (ou `dir`) et un exemple de sortie.

**Recap**  
« Tu as ouvert le terminal et utilisé `pwd` et `ls`. Tu es prêt pour suivre les prochaines vidéos en ligne de commande. »

---

**Notes rédaction**  
- OS : Mac et Linux en priorité pour les démos (commandes `pwd`, `ls`) ; Windows mentionné avec PowerShell / WSL / Git Bash.  
- Pas de `cd` ni d’autres commandes dans ce clip ; rester sur un seul objectif.

---

## Scene breakdown

Une ligne = une scène. Durées en secondes ; total visé = 180 s.

| # | Purpose | Est. duration | Component(s) | Contenu à l’écran (résumé) |
|---|---------|---------------|--------------|---------------------------|
| 1 | Titre et contexte | 8 s | TitleCard | Titre : « Pré-requis : terminal et bases ». Sous-titre : « Pour suivre les démos Git ». |
| 2 | Intro / objectif | 15 s | SectionIntro | « Dans les prochaines vidéos on va utiliser Git en ligne de commande. On va voir où ouvrir le terminal et une ou deux commandes de base. » |
| 3 | Step 1 – Ouvrir le terminal | 45 s | CodeAlongStep | Texte : où trouver le terminal (Mac / Linux / Windows). Terminal ou schéma : fenêtre terminal vide ou icône. |
| 4 | Step 2 – pwd | 40 s | CodeAlongStep | Texte : « Tape pwd pour afficher le dossier actuel. » Terminal : `pwd` + sortie type `/Users/...`. |
| 5 | Step 3 – ls (ou dir) | 42 s | CodeAlongStep | Texte : « Tape ls pour lister les fichiers (dir sur Windows). » Terminal : `ls` + sortie type liste de fichiers. |
| 6 | Recap | 20 s | SectionIntro | « Tu as ouvert le terminal, utilisé pwd et ls. Tu es prêt pour les prochaines vidéos. » |
| 7 | Fin / CTA | 10 s | TitleCard | « À suivre : Git vs GitHub » ou logo THP. |

**Total (check)** : 8 + 15 + 45 + 40 + 42 + 20 + 10 = **180 s** = 3 min.

---

## Components needed

D’après le [Component shortlist](video-ai-preparation.md#component-shortlist).

| Component | Used in scene(s) | Notes |
|-----------|------------------|--------|
| TitleCard | 1, 7 | Titre + sous-titre ; fade simple. |
| SectionIntro | 2, 6 | Accroche et recap. |
| CodeBlockWithHighlight | 4, 5 | Affichage terminal (commandes + sortie) ; steps = une commande par step. |
| CodeAlongStep | 3, 4, 5 | Un step = texte court + bloc terminal. |
| ProgressBar | 3–5 (optionnel P1) | Indicateur d’étape (step 1/2/3). |

**Terminal** : les steps montrent du « terminal » (commandes + sortie). Utiliser soit un composant Terminal (ex. `@repo/ui/remotion` Terminal) soit CodeBlockWithHighlight avec un style terminal ; à documenter dans l’implémentation.

---

## Ready for Remotion when

- [x] Script et scene breakdown remplis et relus (ce document).
- [x] Liste des composants conforme au P0 shortlist ; écarts documentés (Terminal vs CodeBlock).
- [x] Durée cible et format conformes aux [Formats](video-ai-preparation.md#video-formats).

**Implémentation** : Composition `Pilot01Prerequis` dans `apps/remotion/src/remotion/compositions/serie-01/`, enregistrée dans `Root.tsx` (id `Pilot01Prerequis`, 5400 frames, 30 fps, 1920×1080).

---

## Review et rendu (après implémentation)

- **Vérification** : Ouvrir Remotion Studio (`bun run dev --filter=remotion`), sélectionner la composition « Pilot01Prerequis », lire la vidéo de bout en bout. Vérifier durée totale (~3 min), lisibilité des textes et du terminal, enchaînement des 7 scènes.
- **Checklist qualité** : [runbook §06](../runbooks/video-ai-development.md) — code lisible, rythme adapté, pas de régression pédagogique.
- **Rendu** : Suivre [runbooks/remotion](../runbooks/remotion.md) pour la commande `remotion render` (format, codec, sortie).
