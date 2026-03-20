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
updated: 2026-03-20
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

## Ready for Remotion when

- [ ] Script and scene breakdown are filled in and reviewed.
- [ ] Text role mapping table is complete and matches canonical skill/matrix.
- [ ] Component list matches P0 (and any P1) from the shortlist; gaps are documented.
- [ ] Target duration and format are consistent with [Formats](../video-ai-preparation/video-ai-preparation.md#video-formats).
- [ ] [Checklist visuelle THP Solarpunk](thp-solarpunk-visual-checklist.md) passée (ou équivalent documenté dans l’outline).
- [ ] **Agent** : `bun run bootstrap:agents` exécuté si besoin ; skills **thp-video-generation**, **thp-solarpunk-visual** et **remotion-best-practices** sous `.cursor/skills/` ; session ouverte avec la **phrase type** [§08 — Triggers agent](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo).
- [ ] Any exception to text-role matrix is documented in this outline and scheduled for §07 feedback.

Only then create or edit compositions in `apps/remotion` and use components from `packages/remotion-lib`.
