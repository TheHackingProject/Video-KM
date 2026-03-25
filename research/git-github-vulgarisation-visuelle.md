---
title: "Recherche visuelle — Git & GitHub (vulgarisation)"
type: research
diataxis: explanation
status: draft
area: video-ai
tags:
  - research
  - git
  - github
  - vulgarisation
  - visuals
  - serie-01
created: 2026-03-25
updated: 2026-03-25
related:
  - "[[research/soul-recherche-visuelle]]"
  - "[[video-ai-preparation/serie-01-git-github]]"
  - "[[video-ai-preparation/pilot-01-prerequis-outline]]"
---

# Git & GitHub — vulgarisation visuelle (Soul série 01)

Fichier **vivant** : chaque passe de recherche ajoute un bloc **Incrément** ou enrichit une section. Les bullets **À tester en scène** sont destinés à être copiés en raccourci dans les `pilot-*-outline.md` (section cues visuels).

---

## 1. Distinction Git vs GitHub (déjà traitée en pilot 02 — capitaliser)

| Idée | Image mentale grand public | Piste écran (non exclusif) |
|------|----------------------------|----------------------------|
| Git = outil **local** | “Machine à remonter le temps **sur ton PC**” / boîte à souvenirs sur l’étagère | Split : **ta machine** (terminal, dossier) vs **nuage abstrait** plus tard |
| GitHub = **hébergement + social** | “Le coffre partagé / la place du village où tu exposes ta branche” | Deux panneaux `ComparisonTable` ou carte “Local” vs “Distant” avec **même** picto dépôt des deux côtés pour montrer la **copie** |

**À éviter** : deux logos seuls sans mouvement ; le spectateur confond encore “site” et “logiciel”.

---

## 2. Commit — au-delà du texte “sauvegarde datée”

- **Métaphore forte** : **point de sauvegarde dans un jeu** (déjà script pilot 03) → visuel : **timeline** avec un **marqueur** qui “pop” quand la VO dit “instant T”.
- **Renfort visuel** : mini **calendrier / horloge** stylisée (pas une vraie UI OS) attachée au nœud “Commit” du `FlowChart`.
- **À tester en scène** : faire apparaître **3 points** sur une ligne (état A → état B → état C) quand on dit “historique”, **avant** d’afficher le mot “commit”.

---

## 3. Branch — parallélisme sans jargon

- **Métaphore** : voie de garage / voie ferrée d’évitement — **deux trains** sur fond **même ville** (même repo) mais **trajets séparés**.
- **Écran** : graphe **2 lignes** (pas seulement horizontal `main → Branche → Commits`) : essayer **vertical** “main stable en haut, branche qui descend” dans une itération future (voir `FlowChart` `links` mode graph).
- **À éviter** : dire “pointer HEAD” sans représentation ; si HEAD est nommé, montrer un **curseur** ou **étiquette** sur la ligne de temps.

---

## 4. Merge — friction cognitive attendue

- Image mentale : **converger deux ruisseaux** / **fusionner deux colonnes de tableau**.
- **Piste** : animation **deux `FlowChart` simplifiés** qui **se rapprochent** (à prototyper en remotion-lib si récurrent).
- **Vulgarisation** : “remettre le travail de la branche **dans** la ligne principale” — verbe **physique** (rentrer, fusionner, réintégrer).

---

## 5. Pull request & review — social avant technique

- Grand public : “**demande d’intégration**” + “**quelqu’un d’autre valide**”.
- **Visuel** : boîte de dialogue stylisée **A propose → B valide** (même `FlowChart` 3 nœuds mais rôles humains : avatar générique / initiales, pas photo réelle).
- **Piège** : écran GitHub réaliste illisible en 1080p sur mobile ; préférer **schéma épuré** + 1 zoom texte max.

---

## 6. Terminal — quand c’est le bon visuel

- **Oui** : dès que la promesse pédagogique est “tu feras la même chose chez toi”.
- **Non** : pour l’**idée pure** (concept intro 45 s) — limiter à **1** fenêtre terminal par clip ou **extraits** très courts ; le reste = schéma.
- **Renfort** : surlignage **mot-clé** dans la sortie (`pwd`, chemin) — déjà aligné avec `Terminal` ; garder **gros contraste** (solarpunk).

---

## 7. Props récurrents série 01 (cohérence “franchise”)

| Prop | Usage |
|------|--------|
| **Ligne du temps** | historique, commits, avant/après merge |
| **Deux colonnes Local / Distant** | Git vs GitHub, push/pull |
| **Carte “dossier”** | working tree, répertoire courant |
| **Badge “Brouillon / Officiel”** | branche vs main (langage non technique) |

Documenter dans les outlines quand un prop est **réutilisé** pour créer une **habitude visuelle** chez le spectateur.

---

## Incrément — 2026-03-25 (bootstrap fichier)

- **Constat** : les pilots série 01 s’appuient fortement sur **texte animé** + un **FlowChart** par clip — correct pour la V1, insuffisant pour “niveau artistique” cible.
- **Direction** : pour chaque **nouveau** pilot (Merge, PR, Fork…), imposer dans l’outline **au moins une scène** dont le **héros visuel** n’est **pas** un paragraphe Typewriter (diagramme animé, comparaison, prop récurrent, split).
- **Prochaine mini-recherche suggérée** : “métaphores merge conflict” (grand public) + ce qui **ne doit pas** être montré (trop technique trop tôt).

---

## Liens utiles (internes)

- [serie-01-git-github](../video-ai-preparation/serie-01-git-github.md)
- [soul-recherche-visuelle](soul-recherche-visuelle.md) — méthode
- `packages/skills/thp-video-generation/references/library-matrix.md` — rôles texte / composants
