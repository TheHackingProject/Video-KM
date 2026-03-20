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
updated: 2026-03-19
related:
  - "[[video-ai-preparation/video-ai-preparation]]"
  - "[[video-ai-preparation/serie-01-git-github]]"
  - "[[reference/video-lifecycle]]"
  - "[[reference/thp-tone-and-theme]]"
---

# Pilot 01 – Pré-requis : terminal et bases

Outline complet pour la vidéo « Pré-requis » de la [Série 01](serie-01-git-github.md). Format 2 – Code demo guided. **Idée unique** : Savoir ouvrir le terminal et exécuter une ou deux commandes de base pour pouvoir suivre les démos Git de la série.

**Références** : ton [thp-tone-and-theme](../reference/thp-tone-and-theme.md) ; formats [video-ai-preparation](video-ai-preparation.md#video-formats) ; pratiques Remotion (skill `remotion-best-practices` : `sequencing.md`, `timing.md`, `transitions.md`).

---

## Pilot metadata

| Field | Value |
|-------|--------|
| **Title** | Pré-requis : terminal et bases |
| **Format** | Code demo guided (Format 2) |
| **Target duration** | **~2 min (120 s)** cible pédagogique ; fourchette Format 2 : 2–5 min si besoin de rallonge |
| **FPS (composition)** | 30 → **3600 frames** pour 120 s |
| **Source** | THP – pré-requis module Git |
| **Paired video** | Série 01 – vidéo 2 (Git vs GitHub) en aval. |
| **Public** | Débutants THP ; pas de prérequis technique. |
| **Idée unique** | Savoir ouvrir le terminal et exécuter une ou deux commandes de base pour pouvoir suivre les démos de la série. |

---

## Rythme, vulgarisation et VO (recherche + pratique)

Principes utiles pour que la vidéo reste **vivante** sans surcharge cognitive :

| Levier | Application pour ce pilot |
|--------|---------------------------|
| **Fréquence des « beats »** | Chaque **~8–15 s** (clip court), faire apparaître un **changement visible** (sous-titre, pill OS, ligne terminal, pictogramme) — pas seulement la voix. |
| **Densité vs compréhension** | Une idée par phrase ; après chaque commande, **pause visuelle** (~0,5–1 s) sur la sortie avant la suite. |
| **Rythme oral** | ~**130–150 mots/min** ; pour **~2 min**, viser **~260–320 mots** tout compris — **serrer** les phrases ou montrer pills/OS **pendant** la VO pour ne pas dépasser. |
| **Motion** | Micro-transitions courtes (≈ **10–15 frames** @ 30 fps) entre scènes ; entrées en **spring** doux (`damping: 200` = sans rebond) pour le texte important. |

Ces repères s’alignent avec les usages courants en motion design pédagogique (alternance mouvement / **hold** sur l’info clé, éviter le monotone). Pour un **prérequis** à **idée unique**, une cible **~90–120 s** évite la « démo qui s’étire » tout en restant dans le Format 2 (2–5 min) si tu dois rallonger plus tard.

---

## Script (VO + texte à l’écran)

Structure : accroche → objectif → étapes (phrase simple + analogue si utile) → recap. **Tutoiement**, ton direct et encourageant ([thp-tone-and-theme](../reference/thp-tone-and-theme.md)).

---

### Intro (courte)

**VO** : « Salut ! Dans les prochaines vidéos, on va taper des commandes Git dans le **terminal**. Rien de sorcier : aujourd’hui, on ouvre cette fenêtre et on teste deux commandes ultra simples. »

**Écran** : titre + une ligne type « 2 commandes pour être prêt ».

---

### Objectif

**VO** : « À la fin, tu sauras **où** ouvrir le terminal sur ton ordi, et **à quoi servent** `pwd` et `ls` — pour toujours savoir *dans quel dossier tu es* et *ce qu’il contient*. »

**Écran** : mots-clés **où** / **pwd** / **ls** (apparition échelonnée, pas un pavé).

---

### Step 1 – Ouvrir le terminal

**VO** : « Le terminal, en clair, c’est une **fenêtre de texte** où tu parles à l’ordinateur en une ligne : tu tapes, tu valides, ça répond. »

**VO (OS)** : « **Sur Mac** : Spotlight ou menu → “Terminal”, ou Utilitaires. **Sur Linux** : souvent *Ctrl+Alt+T*, ou “Terminal” dans le menu. **Sur Windows** : PowerShell ou l’invite de commandes marchent ; pour coller au cours comme sur Mac/Linux, installe **Git Bash** ou **WSL** — on le redira dans les ressources. »

**Écran** : les trois OS en **pills** ou cartes courtes (icône + 1 ligne) ; pas un long paragraphe. Option : mini schéma « fenêtre noire + curseur ».

---

### Step 2 – Première commande : où je suis ? (`pwd`)

**VO** : « Une fois le terminal ouvert, tape **`pwd`** puis Entrée. En anglais ça veut dire *print working directory* : en français, “affiche le dossier dans lequel je travaille **maintenant**”. C’est ton **repère** : avant de lancer Git, tu veux savoir *où* tu es sur le disque. »

**VO** : « Tu vois un chemin du style `/Users/…` ou `/home/…` : c’est normal, c’est **ta position actuelle**. »

**Écran** : commande `pwd` + sortie exemple ; surligner le chemin 1–2 s.

---

### Step 3 – Lister le contenu (`ls` / `dir`)

**VO** : « Deuxième commande : **`ls`** puis Entrée — *list*. Elle affiche **fichiers et dossiers** du répertoire courant. Sur Windows PowerShell, **`dir`** fait pareil. »

**VO** : « Tu n’as pas besoin de tout comprendre tout de suite : l’essentiel, c’est **voir** que le terminal te **répond** avec une liste. »

**Écran** : `ls` ou `dir` + liste fictive lisible (3–5 lignes max pour la démo).

---

### Recap

**VO** : « Récap : tu sais ouvrir le terminal, tu as vu **`pwd`** pour le dossier courant et **`ls`** pour son contenu. Tu peux enchaîner avec la suite sur Git — on te guide pas à pas. »

**Écran** : les 3 points en une ligne ou mini FlowChart « Terminal → pwd → ls → prêt ».

---

### Fin / CTA

**VO** : « À suivre : **Git vs GitHub**. À tout de suite ! »

**Écran** : titre épisode suivant ou logo THP.

---

### Notes rédaction

- Mac / Linux en **priorité** pour les exemples (`pwd`, `ls`) ; Windows explicitement nommé (PowerShell, `dir`, Git Bash / WSL).
- Ne pas introduire `cd` ni d’autres commandes dans ce clip.
- Éviter le jargon non dit : toujours **une phrase de français** avant l’acronyme anglais.
- **Version ~2 min** : si la VO dépasse le chrono, couper une répétition ou faire apparaître **Mac / Linux / Windows** à l’écran **sans** tout redire à l’oral.

---

## Scene breakdown (timings + frames @ 30 fps)

Une ligne = une **séquence** Remotion (`Series.Sequence` ou `Sequence from={…}`). Les **sous-beats** sont des repères pour stagger les animations *à l’intérieur* de la scène (voir section suivante).

**Pourquoi des durées plus courtes** : pour un clip « une compétence », les repères pédagogiques et motion (changements visuels fréquents, pas d’attente inutile sur un seul plan) donnent de meilleurs résultats en **~2 min** qu’en **3 min** avec les mêmes infos — sans perdre les **holds** courts sur `pwd` / `ls`.

| # | Purpose | Durée (s) | Frames | `from` cumulé (frame) | Contenu écran (résumé) |
|---|---------|-----------|--------|------------------------|-------------------------|
| 1 | Titre et contexte | 5 | 150 | 0 | TitleCard ; sous-titre « 2 commandes pour suivre Git ». |
| 2 | Intro + objectif | 14 | 420 | 150 | Accroche + objectif ; mots-clés échelonnés (pas deux longs paragraphes). |
| 3 | Step 1 – Ouvrir le terminal | 32 | 960 | 570 | Analogie courte ; pills Mac / Linux / Windows en **parallèle** de la VO. |
| 4 | Step 2 – `pwd` | 26 | 780 | 1530 | FR + `pwd` ; terminal ; **hold ~5–6 s** sur le chemin. |
| 5 | Step 3 – `ls` / `dir` | 26 | 780 | 2310 | `ls` + liste ; `dir` ; **hold ~5–6 s** sur la liste. |
| 6 | Recap | 12 | 360 | 3090 | 3 puces ou mini flux ; une phrase VO max. |
| 7 | Fin / CTA | 5 | 150 | 3450 | « À suivre : Git vs GitHub ». |

**Total** : 5 + 14 + 32 + 26 + 26 + 12 + 5 = **120 s** = **3600 frames**.

### Sous-beats (exemple, scène 4 – `pwd`, 26 s ≈ 780 f)

| Phase | ~s | Notes animation |
|-------|-----|-----------------|
| Bandeau step + vulgarisation | 0–5 | `SceneHeader` / bandeau + une phrase clé (`TextReveal` court). |
| Commande + sortie | 5–14 | `Terminal` : `command` puis `output`, `delay` bref entre les deux. |
| Hold lecture chemin | 14–21 | Peu de mouvement ; surlignage du chemin. |
| Pont étape 3 | 21–26 | `ProgressBar` 2/3 ; transition **10–12 f**. |

Adapter de la même façon les scènes 3 et 5 (toujours : **expliquer → montrer → laisser lire**).

---

## Rendu artistique (réf. `DemoShowcaseSolarpunk.tsx` + Remotion)

Inspiration concrète dans le repo : [`packages/ui/src/DemoShowcaseSolarpunk.tsx`](../../../packages/ui/src/DemoShowcaseSolarpunk.tsx) — arrière-plans en dégradés, `ParticleField` discret, `SceneHeader` (numéro / titre / mot-clé), `Terminal` avec `lines[]` et `delay`, `FlowChart` pour enchaîner des idées, `ProgressBar`, cartes avec `startFrame` décalés.

### Pistes pour Pilot 01 (cohérentes THP, pas obligatoire « solarpunk »)

| Élément | Usage |
|---------|--------|
| **Fond** | Dégradé radial ou linéaire sobre (thème cours) ; `ParticleField` **opacité basse** (ex. 0,15–0,25) pour éviter la « diapo morte » sans distraire la lecture du terminal. |
| **En-tête de scène** | `SceneHeader` : `sceneNumber` 1…7 ou regroupement (ex. intro = 1/4, steps = 2/4…) ; `keyword` court (`OUVRIR`, `PWD`, `LS`, `RÉCAP`). |
| **Terminal** | Composant [`Terminal`](../../../packages/ui/src/lib/remotion/code/Terminal.tsx) : tableau `lines` avec `type: command | output | success` et **`delay` en frames** entre lignes ; `typeSpeed` modéré ; **prompt** lisible. |
| **Progression pédagogique** | `ProgressBar` pendant les steps 3–5 (ex. « Étape 1/3 ») pour ancrer le rythme. |
| **Concepts enchaînés** | Option : petit `FlowChart` horizontal « Ouvre le terminal → tape la commande → lis la réponse » sur l’intro ou le recap (même pattern que la scène *Code* du showcase). |
| **Texte** | `WordByWord` ou `TextReveal` pour **une seule phrase clé** par scène (pas tout le script) ; éviter la surcharge. |
| **Transitions** | Entre séquences : fade court ou `TransitionSeries` (cf. skill Remotion `transitions.md`) ~**12–15 frames** ; `premountFor={1*fps}` sur les `Sequence` pour éviter les flashs au montage (`sequencing.md`). |
| **Mouvement** | Entrées : `spring({ config: { damping: 200 } })` pour un rendu net sans rebond (`timing.md`). |

### Checklist implémentation Remotion

- [ ] `Series` ou `Sequence` avec durées = tableau ci-dessus (**3600 frames** total @ 30 fps).
- [ ] `premountFor` sur chaque enfant de timeline qui charge du texte ou du terminal.
- [ ] `useCurrentFrame()` **local à chaque `Sequence`** pour les offsets d’animation (comme commentaire dans le showcase).
- [ ] Après chaque sortie terminal importante : **≥ 15–30 frames** de « hold » lisible avant la transition suivante.

---

## Components needed

D’après le [Component shortlist](video-ai-preparation.md#component-shortlist), enrichi par les patterns déjà dans `@repo/ui/remotion` (showcase).

| Component | Scène(s) | Notes |
|-----------|----------|--------|
| TitleCard | 1, 7 | Titre + sous-titre ; entrée fade + léger translateY. |
| SectionIntro | 2, 6 | Accroche, objectif, recap. |
| SceneHeader | 2–6 (optionnel) | Aligné sur DemoShowcase : contexte + keyword. |
| CodeAlongStep | 3, 4, 5 | Texte court + zone terminal / visuel OS. |
| Terminal | 4, 5 | Lignes + delays ; typewriter ; réutiliser le pattern `terminalLines` du showcase. |
| CodeBlockWithHighlight | (fallback) | Si Terminal indisponible : style terminal. |
| ProgressBar | 3–5 | P1 fortement recommandé pour la clarté. |
| FlowChart | 2 ou 6 | Optionnel : fil narratif en 3 nœuds. |
| ParticleField | fond | Optionnel, discret. |
| WordByWord / TextReveal | 2, 4, 5 | Une phrase clé par scène max. |

---

## Ready for Remotion when

- [x] Script et scene breakdown remplis et relus (ce document).
- [x] Liste des composants conforme au P0 shortlist ; écarts documentés (Terminal vs CodeBlock).
- [x] Durée cible et format conformes aux [Formats](video-ai-preparation.md#video-formats).
- [ ] Storyboard ou capture Studio : vérifier holds sur `pwd` / `ls` et lisibilité mobile (scale terminal).

**Implémentation** : Composition `Pilot01Prerequis` dans `apps/remotion/src/remotion/compositions/serie-01/`, enregistrée dans `Root.tsx` (id `Pilot01Prerequis`, **3600 frames**, 30 fps, 1920×1080).

**Historique** :

- **Retour 2026-03** : « Pas assez vivant, terminal pas animé. » → Terminal avec typewriter et timings ; animations d’entrée sur les cartes.
- **2026-03-19** : Script enrichi (vulgarisation, analogies, OS explicites), breakdown + sous-beats + frames, section rythme/recherche, rendu artistique aligné `DemoShowcaseSolarpunk` et skill Remotion (`sequencing`, `timing`, `transitions`).
- **2026-03-19 (2)** : Cible durée **120 s / 3600 frames** — scènes raccourcies (bonnes pratiques prérequis + rythme visuel) ; implémentation Remotion alignée.

---

## Review et rendu (après implémentation)

- **Vérification** : Remotion Studio (`bun run dev --filter=remotion`), composition « Pilot01Prerequis », lecture bout en bout ; durée **3600 frames**, lisibilité texte/terminal, **pauses** courtes après sorties.
- **Checklist qualité** : [runbook §06](../runbooks/video-ai-development.md) — rythme, pas de surcharge cognitive.
- **Rendu** : [runbooks/remotion](../runbooks/remotion.md) pour `remotion render`.

---

## Outils / sources utilisés pour cette révision

- Recherche web : rythme pédagogique, alternance beats / holds, ordres de grandeur motion (transitions courtes, pauses sur l’info clé).
- Repo : `DemoShowcaseSolarpunk.tsx`, `Terminal`, skill **remotion-best-practices** (`sequencing.md`, `timing.md`, `transitions.md`).
