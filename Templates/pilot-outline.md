---
title: "Pilot Video Outline (Template)"
type: documentation
status: draft
area: video-ai
tags:
  - video-ai
  - pilot
  - thp
  - template
created: 2026-03-12
updated: 2026-03-28
related:
  - "[[video-ai-preparation/video-ai-preparation]]"
  - "[[reference/solarpunk-theme-decisions]]"
  - "[[Templates/thp-solarpunk-visual-checklist]]"
  - "[[meta/thp-video-generation-skill]]"
  - "[[meta/thp-solarpunk-visual-skill]]"
  - "[[runbooks/video-ai-development]]"
---

# Pilot Video Outline (Template)

Template for the first (and subsequent) pilot videos. **Copy or rename this file per pilot** (e.g. into `video-ai-preparation/pilot-01-http-intro-outline.md` or keep in a working folder). Fill it in with a real THP course extract **before** writing any Remotion composition. See [Formats](../video-ai-preparation/video-ai-preparation.md#video-formats) and [Component shortlist](../video-ai-preparation/video-ai-preparation.md#component-shortlist) for context.

**Identité visuelle THP** : toutes les vidéos suivent le kit **Solarpunk dark** ([décisions](../reference/solarpunk-theme-decisions.md)). Avant livraison, parcourir [checklist visuelle Solarpunk](thp-solarpunk-visual-checklist.md).

**Montée en gamme visuelle** : au-delà du texte animé, ce template impose une **intention schématique** (graphe + révélation + polish) pour les formats courts — voir [thp-video-generation skill](../../../packages/skills/thp-video-generation/SKILL.md) (*Schématiser l’idée*).

### Avec un agent Cursor (ou équivalent)

Pour que la procédure **remonte** et soit appliquée quand c’est pertinent :

1. **Bootstrap une fois par machine / clone** : depuis la racine Video-AI, `bun run bootstrap:agents` (sous-modules Remotion + liens `.cursor/skills/`). Voir [runbook §08](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo) et [`.cursor/environment.json`](../../../.cursor/environment.json) pour les Background Agents.
2. **Charger explicitement trois Agent Skills** dès qu’on touche au **script**, au **découpage de scènes**, aux **timings** ou au **code Remotion** (Cursor ne garantit pas l’auto-sélection) :
   - **[THP video generation](../meta/thp-video-generation-skill.md)** — choix de bloc THP (texte, code, transitions, diagrammes, 3D), taxonomie texte, `Sequence`, boucle Storybook → démo → doc.
   - **[THP Solarpunk visual](../meta/thp-solarpunk-visual-skill.md)** — `solarTheme`, contraste, motion, tokens.
   - **remotion-best-practices** (officiel Remotion, même arbre que `packages/skills/remotion-best-practices`) — **rules** dans `packages/skills/remotion-best-practices/rules/*.md` selon la tâche ; carte des triggers : [runbook §08](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo) (*Triggers agent*).
3. **Phrase type (forte)** à coller en tête de session :

   *« Applique **thp-video-generation**, **thp-solarpunk-visual** et **remotion-best-practices**. Ouvre les rules Remotion listées dans video-ai-development §08 (triggers) pour cette passe. »*

**Itération** : avant de rééditer le **script** ou le **découpage de scènes** dans une copie pilote, vérifier si ce template a changé et aligner la copie — procédure : [video-ai-preparation — Template sync before script edits](../video-ai-preparation/video-ai-preparation.md#template-sync-before-script-edits) et [video-ai-development §03b](../runbooks/video-ai-development.md) (*Itération — synchroniser le template avant le script*).

---

## Pilot metadata

| Field | Value |
|-------|--------|
| **Title** | [e.g. "What is HTTP?"] |
| **Format** | Concept intro \| Code demo guided |
| **Target duration** | [e.g. 45 s \| 3 min] — from [Formats](../video-ai-preparation/video-ai-preparation.md#video-formats) |
| **Source** | [THP module/lesson name or link to course content] |
| **Paired video** | [If Code demo: link to Concept intro on same topic. If Concept intro: optional "follow-up" Code demo.] |

---

## Format 1 vs Format 2+ — graphe et storyboard

| Règle | Détail |
|-------|--------|
| **Format 1** (concept court, typ. 30–60 s) | **Graphe conceptuel** + **Storyboard de révélation** ci-dessous sont **obligatoires** avant code Remotion pour l’idée principale (sauf exception documentée en §07). |
| **Format 2+** (code demo guidé, long, multi-chapitres) | Remplir graphe + storyboard **si le sujet le justifie**. Sinon : sous chaque section concernée, une ligne **N/A —** + **justification en une phrase** (pas de checklist lourde sans gain visuel). |

**Principe** : éviter l’« usine à cases » — peu d’items, **lisibles**, appliqués là où la valeur est claire.

---

## Script

[Paste or link the script here. Keep it short for the first pilot.]

### Text role mapping (required, before code)

Map each visible text block to a role and target component using the canonical matrix in:

- `packages/skills/thp-video-generation/SKILL.md`
- `packages/skills/thp-video-generation/references/library-matrix.md`

| Text block | Role id | Target component | Timing/color notes | Exception reason (if any) |
|------------|---------|------------------|--------------------|---------------------------|
| [e.g. intro title] | [e.g. ROLE_INTRO_HERO] | [e.g. GlitchText] | [duration, highlight, cps] | [required only if diverging] |
| [e.g. subtitle] | [e.g. ROLE_INTRO_SUBTITLE] | [e.g. TextReveal] | [...] | [...] |

**Structure hint** (align with chosen format):

- **Concept intro**: hook (1–2 sentences) → one concept (2–4 sentences) → optional micro-recap (1 sentence).
- **Code demo guided**: short intro → concept reminder → steps (for each step: 1–2 sentences + what appears in code) → recap.

---

## Scene breakdown

One row per scene. Map each scene to a purpose and to components from [Component shortlist](../video-ai-preparation/video-ai-preparation.md#component-shortlist).

| # | Purpose | Est. duration | Component(s) |
|---|---------|---------------|----------------|
| 1 | [e.g. title card] | [e.g. 3 s] | TitleCard |
| 2 | [e.g. hook / section intro] | [e.g. 5 s] | SectionIntro |
| 3 | [e.g. concept or code step] | [e.g. 15 s] | ConceptSlide \| CodeAlongStep |
| … | … | … | … |

**Total (check)**: [sum] ≈ target duration.

---

## Message visuel dominant et hero object (par scène)

**Règle** : **une scène = un message visuel dominant.** Si la scène inclut un graphe, **le graphe porte le sens principal** — pas une illustration décorative d’une phrase.

| # | Message visuel dominant (une phrase) | Hero (objet principal) | Secondaire (support, ex. titre, sous-texte) |
|---|--------------------------------------|------------------------|---------------------------------------------|
| 1 | [e.g. marque la leçon en titre glitch] | GlitchText titre | TextReveal sous-titre |
| 2 | … | … | … |

---

## Graphe conceptuel (avant Remotion)

*(Format 1 : **obligatoire**. Format 2+ : si pertinent ; sinon **N/A —** + justification.)*

**Nœuds** (viser 3–7) : libellé court + forme ou icône envisagée.

- [e.g. **Commit** — icône ▸]
- …

**Arêtes** (graphe orienté) :

| Source | Cible | Relation (ex. sert à, ne doit jamais, dérive de) |
|--------|-------|--------------------------------------------------|
| [nœud A] | [nœud B] | [libellé] |

---

## Storyboard de révélation du schéma

*(Format 1 : **obligatoire** lorsque le graphe porte la leçon. Format 2+ : N/A possible avec justification.)*

Pour chaque beat visuel majeur du schéma : **quoi** apparaît **quand**, aligné sur la VO. Colonnes **Entrée / hold / sortie** optionnelles mais recommandées pour 1–2 beats clés (rythme premium).

| Beat | Début (frame ou s) | Fin (frame ou s) | Élément révélé (nœud / trait / libellé flèche) | Lien VO (phrase ou timestamp script) | Entrée / hold / sortie (optionnel) |
|------|--------------------|------------------|-----------------------------------------------|----------------------------------------|-------------------------------------|
| 1 | | | | | |

**Idées de polish** (réf. skill) : flèche en **deux temps** (trait puis libellé) ; **nœud actif** (bordure / scale / ombre) sync VO ; en fin de section, graphe qui **se replie ou décale** plutôt que cut brutal.

---

## Polish visuel (checklist courte)

Cocher ce qui s’applique à ce pilote (pas besoin de tout remplir si la vidéo est minimaliste).

- [ ] **Stagger** : décalage de quelques frames entre éléments d’un même groupe si la scène est dense.
- [ ] **Élément actif** aligné sur la phrase VO (surbrillance, pulse léger, bordure — tokens `solarTheme`).
- [ ] **Transition de section** : le schéma ne disparaît pas brutalement si un replis / slide est faisable.
- [ ] **Respiration** : pour les beats critiques, durées **entrée → hold → sortie** notées (ici ou dans `*-content.ts`).
- [ ] **Cartes nœud** : au plus **deux niveaux** d’information par carte (titre + sous-texte bref).

---

## Cues visuels / Soul (vulgarisation)

Avant ou en parallèle du script : consulter le fichier **research** du sujet (voir [Soul — recherche visuelle](../research/soul-recherche-visuelle.md)). Pour **chaque scène**, noter **1–3 bullets** : métaphore, prop, schéma prioritaire — ce qui doit être compris **sans** lire tout le texte.

| # | Cues visuels / Soul (résumé) | Composant ou artefact cible |
|---|------------------------------|-----------------------------|
| 1 | [e.g. split Local vs nuage] | ComparisonTable \| … |
| 2 | … | … |

---

## Components needed

From [Component shortlist](../video-ai-preparation/video-ai-preparation.md#component-shortlist), tick which components this pilot uses. If the pilot reveals a missing piece, note it and consider updating the shortlist.

| Component | Used in scene(s) |
|-----------|------------------|
| TitleCard | [e.g. 1] |
| SectionIntro | [e.g. 2] |
| CodeBlockWithHighlight | [e.g. 4, 5] |
| ConceptSlide | [e.g. 3] |
| CodeAlongStep | [e.g. 4, 5, 6] |
| *(others as needed)* | |

---

## Static UI gate (before Remotion) {#static-ui-gate-before-remotion}

**But** : éviter le piège « on anime du texte dans Remotion sans avoir figé les briques statiques ». La DA et la lisibilité passent par **sélection ou création** dans `packages/ui` (puis animation dans `remotion-lib` ou composition), pas par plus de `Sequence` texte.

**Règle** : avant le premier commit ciblant `apps/remotion` ou `packages/remotion-lib` pour ce pilote, chaque scène a une ligne ci-dessous.

| # scène | Besoin visuel (une phrase) | Source | Action |
|--------|----------------------------|--------|--------|
| 1 | [e.g. titre hero + sous-titre] | `@repo/ui` — [export] | Réutiliser — story existante : [fichier `.stories.tsx`] |
| 2 | [e.g. graphe local → remote] | `FlowChart` / schéma / autre | Réutiliser **ou** nouveau bloc UI + story **ou** asset diagramme (voir runbook §03b 3bis) |
| … | … | … | … |

**Colonne « Action »** — valeurs attendues : `Réutiliser` (préciser export + story si disponible) \| `Créer` (nouveau fichier `packages/ui` + `*.stories.tsx` avant Remotion) \| `N/A —` + **raison** (ex. clip audio-only ; à valider en §07).

**Contrôle qualité** : au moins **une** ligne ne doit **pas** être « texte seul animé » (pas seulement un rôle texte du matrix sans support carte/schéma). Si tout est texte : une ligne d’exception en bas + revue process.

---

## Ready for Remotion when

- [ ] Script and scene breakdown are filled in and reviewed.
- [ ] **Format 1** : **Graphe conceptuel** + **Storyboard de révélation** remplis (ou exception documentée pour §07). **Format 2+** : N/A justifié si sections non applicables.
- [ ] **Message dominant + hero / secondaire** : une ligne par scène (ou justification si scène purement titre).
- [ ] Au moins **une scène** où le **schéma (ou prop non-texte) est le héros** pour les formats courts — pas seulement « gros Typewriter + petit schéma ».
- [ ] **Cues visuels / Soul** : fichier `KM/Docs/research/<sujet>-vulgarisation-visuelle.md` consulté ou mis à jour.
- [ ] **Polish visuel** : items pertinents cochés ou explicitement reportés avec raison.
- [ ] Text role mapping table is complete and matches canonical skill/matrix.
- [ ] **Static UI gate** : tableau rempli (toutes les scènes) ; toute ligne `Créer` a un chemin `packages/ui` + story prévu ou déjà mergé avant la composition.
- [ ] Component list matches P0 (and any P1) from the shortlist; gaps are documented.
- [ ] Target duration and format are consistent with [Formats](../video-ai-preparation/video-ai-preparation.md#video-formats).
- [ ] [Checklist visuelle THP Solarpunk](thp-solarpunk-visual-checklist.md) passée (ou équivalent documenté dans l’outline).
- [ ] **Agent** : `bun run bootstrap:agents` exécuté si besoin ; skills **thp-video-generation**, **thp-solarpunk-visual** et **remotion-best-practices** sous `.cursor/skills/` ; session ouverte avec la **phrase type** [§08 — Triggers agent](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo).
- [ ] Any exception to text-role matrix is documented in this outline and scheduled for §07 feedback.

Only then create or edit compositions in `apps/remotion` and use components from `packages/remotion-lib`.
