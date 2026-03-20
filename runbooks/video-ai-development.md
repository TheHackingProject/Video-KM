---
title: "Video-AI Development Runbook"
type: runbook
diataxis: how-to
status: published
area: video-ai
tags:
  - runbook
  - video-ai
  - development
  - workflow
created: 2026-03-12
updated: 2026-03-19
related:
  - "[[00-architecture]]"
  - "[[reference/video-lifecycle]]"
  - "[[video-ai-preparation/video-ai-preparation]]"
  - "[[explanation/video-ai-vision]]"
  - "[[runbooks/remotion]]"
  - "[[reference/solarpunk-theme-decisions]]"
---

# Video-AI Development Runbook

Single operational entry point for developing and evolving Video-AI: purpose, context, workflow, conventions, quality, and deployment. For rendering/ops (e.g. Lambda, batch render), see a future runbook: `video-ai-rendering.md`.

---

## 01 – Project purpose and scope

Video-AI produces and evolves **pedagogical web development videos** for The Hacking Project (THP).

- **v1**: Team-created base of videos; compositions in this repo, rendered and integrated into the THP app.
- **v2**: Feedback and AI-driven improvement of existing videos.
- **v3**: Guided autonomous regeneration of content within team-defined guardrails.

For full scope and audience, see [reference/video-lifecycle](reference/video-lifecycle.md#role-and-audience). For long-term vision and v1/v2/v3, see [explanation/video-ai-vision](explanation/video-ai-vision.md).

---

## 02 – Key context

- **Video lifecycle**: From idea to prod and back: idea → script → composition → review → render → THP integration → feedback → iteration. Full detail (steps, who does what, where in repo): [reference/video-lifecycle](reference/video-lifecycle.md).
- **Preparation (write before code)**: Formats and pilot outlines live in [video-ai-preparation](video-ai-preparation/video-ai-preparation.md).
- **Monorepo**: Turborepo; apps live under `apps/` (e.g. `apps/remotion`), shared packages under `packages/` (e.g. `packages/remotion-lib`, `packages/ui`).
- **Remotion**: Used to author video compositions. Compositions are registered in `apps/remotion/src/remotion/Root.tsx` and live under `apps/remotion/src/remotion/compositions/`. Rendered outputs are consumed by the THP app.
- **Three levels**: Static UI (`packages/ui`), animated building blocks (`packages/remotion-lib`, and currently `packages/ui/src/lib/remotion`), and compositions (`apps/remotion`). See [00-architecture](00-architecture.md) and [runbooks/remotion](remotion.md).

---

## 03 – Development workflow

**Goal**: Add or edit a THP course video.

Before building new components or compositions, define formats and component plans in [video-ai-preparation](video-ai-preparation/video-ai-preparation.md#video-formats) and [component shortlist](video-ai-preparation/video-ai-preparation.md#component-shortlist).

1. **Clone and install**  
   From repo root: `git clone --recurse-submodules <repo-url>`, then `bun install`.

2. **Run dev**  
   - **Remotion Studio** : `bun run dev --filter=remotion` (or `cd apps/remotion && bun run dev`).  
   - **Storybook** (`@repo/ui`) : **port 6006** — `bun run storybook` from repo root, ou `http://<IP>:6006` sur le réseau local. **Pas** le port 3000 (c’était l’ancien `next dev` du dossier storybook ; le script `dev` de `apps/storybook` lance maintenant Storybook). Détail : [apps/storybook/README](../../apps/storybook/README.md).

3. **Where to work**  
   - **Compositions** (full video “scenes”): create or edit under `apps/remotion/src/remotion/compositions/` (e.g. `demos/`, or domain folders like `marketing/`, `onboarding/`). Register new compositions in `apps/remotion/src/remotion/Root.tsx`.
   - **Animated primitives/blocks**: add under `packages/remotion-lib/src/` (primitives, blocks, sections). Use `@repo/remotion-lib` from compositions when needed.

4. **Commands**  
   - Remotion: `bun run dev --filter=remotion`  
   - Storybook: `bun run storybook` (localhost:6006)  
   - Build: `bun run build` (root)  
   - Render: see [runbooks/remotion](remotion.md) for `remotion render`, stills, and codecs.

Do not duplicate full Remotion command reference here; link to the Remotion runbook.

---

## 03b – Procédure détaillée (nouvelle vidéo)

**Fichier de référence pour la procédure** : cette section. À **mettre à jour au fil des retours** (voir [07 – Amélioration du process](#07--amélioration-du-process)). Ordre contractuel :

1. **Préparation (write before code)**  
   - Définir ou réutiliser la série dans [video-ai-preparation](../video-ai-preparation/serie-01-git-github.md) (ou équivalent).  
   - Pour chaque vidéo : copier le [template pilot outline](../Templates/pilot-outline.md) dans `video-ai-preparation/`, remplir **métadonnées** (titre, format, durée, idée unique), **script** (hook → concept → recap pour Format 1 ; intro → étapes → recap pour Format 2), **découpage de scènes** (durée + composants par scène).  
   - Référence formats et shortlist : [video-ai-preparation](../video-ai-preparation/video-ai-preparation.md). Pour le **ton et le style** des scripts (tutoiement, direct, communauté THP), voir [reference/thp-tone-and-theme](reference/thp-tone-and-theme.md).

2. **Composants statiques (UI + Storybook)**  
   - Créer dans `packages/ui/src/` les composants **statiques** nécessaires (ex. TitleCard, SectionIntro, ConceptSlide, CodeBlockStatic ou briques réutilisables).  
   - Pour Format 2 (code demo) avec **terminal** : prévoir un bloc code/terminal statique (ex. CodeBlockStatic avec style terminal) ; les steps « commande + sortie » sont gérés en scène par CodeAlongStep en remotion-lib.  
   - Ajouter les stories colocated (`*.stories.tsx`) et valider dans Storybook (`bun run storybook` — **port 6006**).  
   - Règle : pas de `useCurrentFrame` ni de timing Remotion dans `packages/ui` ; voir [00-architecture](00-architecture.md#ui-vs-remotion).

3. **Composants animés (remotion-lib)**  
   - Dans `packages/remotion-lib/src/`, créer les composants **animés** qui utilisent `@repo/ui` et ajoutent le timing (useCurrentFrame, Sequence, interpolate).  
   - Exporter depuis `packages/remotion-lib/src/index.ts` (primitives, blocks, sections).

4. **Composition Remotion**  
   - Créer ou modifier une composition sous `apps/remotion/src/remotion/compositions/` qui enchaîne les scènes selon l’outline (durées, props).  
   - Enregistrer dans `apps/remotion/src/remotion/Root.tsx`.

5. **Review et rendu**  
   - Vérifier en Remotion Studio ; checklist qualité [06](#06--quality-review-and-deployment). Rendu : [runbooks/remotion](remotion.md).

---

## 04 – Conventions for video courses

- **Visual identity (THP)**: **Solarpunk dark** is the product default — shared tokens (`packages/theme/solarpunk.tokens.css`), Remotion `solarTheme` / `defaultTheme`, extended palette in `demo-showcase/config.ts`. Decisions and feedback log: [reference/solarpunk-theme-decisions](../reference/solarpunk-theme-decisions.md). Before closing a video or UI slice, run [Templates/thp-solarpunk-visual-checklist](../Templates/thp-solarpunk-visual-checklist.md). **WCAG contrast** is mandatory when changing colors or type sizes. **Icons**: `@repo/ui/icons` (Lucide). Optional future signature SVGs: `packages/ui/src/assets/thp/svg/` (README).
- **Structure**: Prefer a clear flow: intro → concept explanation → code demo → recap.
- **Naming**: Use a consistent scheme, e.g. THP slug + module + lesson (e.g. `react-module-2-lesson-3-intro`). Keep composition IDs and file names aligned so they are discoverable.
- **Remotion primitives**: Reuse building blocks from `@repo/remotion-lib` and `@repo/ui/remotion`. For a detailed list of primitives and usage, see [runbooks/remotion](remotion.md); a dedicated reference doc (e.g. `video-ai/remotion-primitives.md`) may be added later.

---

## 05 – Feedback and iteration loop (future IA)

- **Where feedback lives**: User feedback is intended to be stored and keyed by video/course (e.g. composition ID or THP lesson identifier). Data model and storage are defined in the THP platform or a dedicated service; this repo does not own the persistence layer.
- **How devs use it**: Prioritize which videos to improve or regenerate based on feedback (and later, AI-suggested priorities). Implement changes in compositions or remotion-lib, then re-render and ship.
- **TODO**: When AI/skills and automation exist, add a how-to or runbook section and link it here.

---

## 06 – Quality, review, and deployment

- **Quality checklist for a new video**  
  - Code in compositions and remotion-lib is readable and follows repo conventions.  
  - Pacing and clarity are suitable for the target lesson.  
  - Audio (if any) is clear and consistent.  
  - Video is tested in the THP app (or equivalent consumer) before release.

- **PR review for new videos/components**  
  - **Code review**: Same as for other repo changes (lint, types, structure).  
  - **Pedagogical review**: Content and flow match the course intent; no regressions for learners.

- **Deployment**  
  - Rendered assets are published or deployed according to THP platform process (e.g. upload to CDN, trigger app update). Rendering and ops procedures will be documented in a future `video-ai-rendering.md` runbook when applicable.

---

## 07 – Amélioration du process

- **Où documenter la procédure** : cette section et surtout [03b – Procédure détaillée](#03b--procédure-détaillée-nouvelle-vidéo) sont le **fichier dédié** à la procédure. Pas de document séparé : le runbook Video-AI Development est la source de vérité.
- **Quand mettre à jour** : dès qu’une étape change (ex. nouvel outil, nouvel ordre UI → Remotion, nouveau template d’outline), mettre à jour la section 03b et, si besoin, [reference/video-lifecycle](reference/video-lifecycle.md) pour rester cohérent.
- **Comment** : éditer ce runbook directement ; indiquer en 03b les changements notables (ex. « depuis 2026-03 : composants statiques d’abord dans Storybook ») si utile pour la traçabilité.

- **Retours pilots** (à compléter après chaque livraison) :  
  - **Pilot 01 (Pré-requis terminal, 2026-03)** : [outline](../video-ai-preparation/pilot-01-prerequis-outline.md). Composants ajoutés : TitleCard, SectionIntro, ConceptSlide, CodeBlockStatic (UI) ; FadeIn, TitleCardAnimated, SectionIntroAnimated, ConceptSlideAnimated, CodeBlockWithHighlight, CodeAlongStep (remotion-lib). Composition `Pilot01Prerequis` dans `apps/remotion/.../serie-01/`.  
  - **Retour 1 (2026-03)** : « Pas assez vivant, pas d’animations, même le terminal n’est pas animé. » → **Actions** : (1) Utiliser le composant [Terminal](packages/ui/src/lib/remotion/code/Terminal.tsx) de `@repo/ui/remotion` (typewriter, ligne par ligne) pour les scènes terminal au lieu de CodeBlockStatic. (2) Renforcer les animations (FadeIn + translateY, entrées plus dynamiques). (3) Référence ton/script : [reference/thp-tone-and-theme](reference/thp-tone-and-theme.md) (analyse cours Intro week_01) pour aligner le ton des vidéos avec THP.
  - **Thème (2026-03-19)** : THP = **Solarpunk dark** comme thème principal ; `:root` web aligné ; `defaultTheme` Remotion = `solarTheme` ; source CSS `solarpunk.tokens.css` ; icônes via `@repo/ui/icons` (Lucide) ; skill Cursor `thp-solarpunk-visual`. Détail et tableau d’amélioration continue : [solarpunk-theme-decisions](../reference/solarpunk-theme-decisions.md).
