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
updated: 2026-03-23
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

1. **Installer les skills en local** : depuis la racine du repo Video-AI, symlink ou copie vers `.cursor/skills/` (souvent gitignoré) — voir *Cursor install* dans chaque skill et [runbook §08](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo).
2. **Charger explicitement les skills** au début d’une session qui touche au script, au découpage de scènes, ou au code Remotion (Cursor ne garantit pas l’auto-sélection) :
   - **[THP video generation](../meta/thp-video-generation-skill.md)** — choix de bloc (texte, code, transitions, diagrammes, 3D), taxonomie texte, durées de `Sequence`, boucle Storybook → démo → doc.
   - **[THP Solarpunk visual](../meta/thp-solarpunk-visual-skill.md)** — `solarTheme`, contraste, motion, tokens ; à utiliser dès qu’on définit ou retouche des **composants / couleurs / animations**.
3. **Phrase type** dans le chat : *« Applique les skills thp-video-generation et thp-solarpunk-visual pour ce pilot. »*

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
- [ ] Component list matches P0 (and any P1) from the shortlist; gaps are documented.
- [ ] Target duration and format are consistent with [Formats](../video-ai-preparation/video-ai-preparation.md#video-formats).
- [ ] [Checklist visuelle THP Solarpunk](thp-solarpunk-visual-checklist.md) passée (ou équivalent documenté dans l’outline).
- [ ] **Agent** : skills [THP video generation](../meta/thp-video-generation-skill.md) + [THP Solarpunk visual](../meta/thp-solarpunk-visual-skill.md) disponibles sous `.cursor/skills/` (ou équivalent) et **chargés** pour la passe d’implémentation Remotion.

Only then create or edit compositions in `apps/remotion` and use components from `packages/remotion-lib`.
