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
updated: 2026-03-21
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

When **iterating** on script, scene breakdown, or Remotion for an existing pilot, **check** whether [Templates/pilot-outline.md](../Templates/pilot-outline.md) changed and **align** the pilot copy in `video-ai-preparation/` before editing (systematic step — [video-ai-preparation — Template sync](../video-ai-preparation/video-ai-preparation.md#template-sync-before-script-edits) and **§03b step 1bis** below).

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
   - Pour chaque vidéo : copier le [template pilot outline](../Templates/pilot-outline.md) dans `video-ai-preparation/`, remplir **métadonnées** (titre, format, durée, idée unique, **vidéo jumelle** si applicable), **script** (hook → concept → recap pour Format 1 ; intro → étapes → recap pour Format 2), **découpage de scènes** (durée + composants par scène), **composants requis**, checklist **Ready for Remotion** (y compris blocs agents — voir template).  
   - Référence formats et shortlist : [video-ai-preparation](../video-ai-preparation/video-ai-preparation.md). Pour le **ton et le style** des scripts (tutoiement, direct, communauté THP), voir [reference/thp-tone-and-theme](reference/thp-tone-and-theme.md).

1bis. **Itération — synchroniser le template avant le script**  
   Avant de **modifier le script**, le **découpage de scènes** ou le **code Remotion** dans une boucle de retours (V0.x, révisions pédago, etc.) :  
   - Ouvrir le fichier canonique [Templates/pilot-outline.md](../Templates/pilot-outline.md) (champ `updated` du frontmatter + corps).  
   - Comparer avec la copie pilote dans `KM/Docs/video-ai-preparation/` (`git log` / `git diff` sur le template depuis la dernière synchro de ce pilot, ou revue structurelle : métadonnées **Paired video**, table **Components needed**, checklist **Ready for Remotion when**, bloc agents).  
   - **Si le template a changé** : mettre à jour d’abord la copie pilote (sections / cases / consignes manquantes), **puis** refaire l’itération (script → scènes → Remotion) à partir du document à jour.  
   - **Si inchangé** : poursuivre les modifications prévues.  
   Détail et tableau des blocs obligatoires : [video-ai-preparation — Template sync before script edits](../video-ai-preparation/video-ai-preparation.md#template-sync-before-script-edits).

2. **Composants statiques (UI + Storybook)**  
   - Créer dans `packages/ui/src/` les composants **statiques** nécessaires (ex. TitleCard, SectionIntro, ConceptSlide, CodeBlockStatic ou briques réutilisables).  
   - Pour Format 2 (code demo) avec **terminal** : prévoir un bloc code/terminal statique (ex. CodeBlockStatic avec style terminal) ; les steps « commande + sortie » sont gérés en scène par CodeAlongStep en remotion-lib.  
   - Ajouter les stories colocated (`*.stories.tsx`) et valider dans Storybook (`bun run storybook` — **port 6006**).  
   - Règle : pas de `useCurrentFrame` ni de timing Remotion dans `packages/ui` ; voir [00-architecture](00-architecture.md#ui-vs-remotion).

3. **Composants animés (remotion-lib)**  
   - Dans `packages/remotion-lib/src/`, créer les composants **animés** qui utilisent `@repo/ui` et ajoutent le timing (useCurrentFrame, Sequence, interpolate).  
   - Exporter depuis `packages/remotion-lib/src/index.ts` (primitives, blocks, sections).

3bis. **Diagrammes (vidéo très animée : révélation étape par étape)**  
   - Source de vérité recommandée : garder les diagrammes en `.mmd` (Mermaid) dans le repo pour faciliter diff/review/versioning.  
   - Générer un asset SVG (ou PNG) avant intégration Remotion, puis animer la révélation côté composition (séquences, opacité, masques, zoom).  
   - Si l’animation doit être ultra-fine par noeud/liaison, prévoir un SVG structuré en groupes ou une reconstruction partielle en composants React/SVG dans Remotion.  
   - Ce flux évite d’exécuter Mermaid au runtime du rendu vidéo et rend le pipeline plus déterministe.

   **Convention d’emplacement (proposition)**  
   - Fichiers source `.mmd` : à côté de la prépa vidéo, p.ex. `KM/Docs/video-ai-preparation/diagrams/<slug-video>/` (créer le dossier si besoin).  
   - SVG (ou PNG) générés : là où Remotion les charge — p.ex. `apps/remotion/public/diagrams/<slug-video>/` avec [`staticFile`](https://www.remotion.dev/docs/staticfile) ou import statique selon le runbook Remotion.

   **Commande type : Mermaid → SVG (CLI officiel)**  
   Sans ajouter de dépendance au monorepo : exécuter une fois le CLI [@mermaid-js/mermaid-cli](https://github.com/mermaid-js/mermaid-cli) via `bunx` ou `npx`.

   ```bash
   # Depuis la racine du repo (chemins adaptables)
   bunx @mermaid-js/mermaid-cli \
     -i KM/Docs/video-ai-preparation/diagrams/mon-pilot/flow.mmd \
     -o apps/remotion/public/diagrams/mon-pilot/flow.svg
   ```

   Variantes utiles :  
   - PNG : remplacer l’extension de sortie par `.png`.  
   - Fichier de config Mermaid (thème, fond) : option `-c mermaid-config.json` (voir la doc du CLI).  
   - **Thèmes / batch avancés** : skill Agent **pretty-mermaid** sur [skills.sh — imxv/pretty-mermaid-skills](https://skills.sh/imxv/pretty-mermaid-skills/pretty-mermaid) — installation typique : `npx skills add https://github.com/imxv/pretty-mermaid-skills --skill pretty-mermaid` ; puis scripts `render.mjs` / `batch.mjs` dans le dépôt du skill. À utiliser quand le CLI officiel suffit rarement (thèmes intégrés, rendu parallèle) ; sinon rester sur `bunx @mermaid-js/mermaid-cli` ci-dessus. Détail et comparaison : `packages/skills/README.md` (section Mermaid → SVG).

   **Liste de contrôle versionnée (ordre de travail)** : pour chaque vidéo avec diagrammes, maintenir `KM/Docs/video-ai-preparation/diagrams/<slug-video>/ASSET-PIPELINE.md` (source `.mmd` → SVG sous `apps/remotion/public/diagrams/` → Storybook si nouveau bloc statique → démo Remotion si nouveau pattern → doc / matrix). Référence agent (EN) : `packages/skills/thp-video-generation/references/diagram-asset-pipeline.md`. Cela limite les sauts d’étapes côté agent et facilite la revue (« ordre respecté »).

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
- **Pacing, séquences et texte animé (THP / Remotion)** — à appliquer dès qu’une scène paraît « figée » :
  - **Découper** chaque grande scène en **beats** (sous-`Sequence` ou `Series`) plutôt qu’un seul bloc plein écran : enchaînement **setup → reveal → hold → handoff** ; viser un **changement visible** régulier (aligné avec la section *Rythme* des outlines pilot).
  - **Typewriter / mots / reveal / glitch** : réutiliser les patterns de la composition démo `TextDemo` (`Typewriter`, `WordByWord`, `TextReveal`, `GlitchText` depuis `@repo/ui/remotion`). **Source de vérité des rôles** : `packages/skills/thp-video-generation/SKILL.md` + `packages/skills/thp-video-generation/references/library-matrix.md` (matrice unique, sans doublon). Pour le narratif long, privilégier le **typewriter** avec **pauses** entre phrases ; le skill **remotion-best-practices** (`text-animations.md`) rappelle : effet machine à écrire par **slicing** de chaîne, pas par opacité caractère par caractère.
  - **Timings** : centraliser dans un module `*-content.ts` (ou équivalent) les constantes **CPS / charsPerSecond**, **delays** entre lignes terminal, **holds** après une sortie ou une idée clé, pour ajuster sans disperser les nombres magiques dans la composition.
  - **Séquençage** : `premountFor` sur les `Sequence` concernées ; chevauchements légers (quelques frames) entre éléments pour éviter les coupures trop sèches ; réf. skill `sequencing.md` (Series, offsets négatifs si besoin).
  - **Terminal** : garder une **pause lisible** entre commande et sortie, puis un **hold** sur la sortie avant la fin de sous-scène (cohérent avec le tableau *Rythme* des outlines).
  - **Vérification** : lecture Studio bout en bout + conscience des **temps sans mouvement** ; compléter [Templates/thp-solarpunk-visual-checklist](../Templates/thp-solarpunk-visual-checklist.md) pour le reste (contraste, etc.).
  - **Sous-`Sequence` et contenu persistant** : si un calque (texte, pills, etc.) doit **rester à l’écran jusqu’au cut** de la scène parente, la `durationInFrames` de la sous-`Sequence` qui le porte doit couvrir **toute la fin de scène** (typiquement `durationInFrames = duréeScèneParente - from`). Sinon Remotion **démonte** le nœud à la fin de la sous-`Sequence` : le contenu **disparaît avant la fin** de la scène — scène visuellement « incomplète ». Les **chevauchements** entre beats restent possibles via des `from` décalés ; ce qui change est la **durée de vie** du montage, pas seulement le moment d’apparition.
  - **`Serie01SceneShell` `stack` + `FlowChart` sous le texte** : ne pas placer le schéma en `position: absolute` / `bottom` (voir réf. ci-dessous). En flux : colonne **`justifyContent: "center"`** sur un wrapper `flex:1` pour centrer le **bloc** texte + schéma ; **pas** de `flex:1` **entre** le texte et le `FlowChart` (sinon le schéma colle à la barre de progression et le milieu de l’écran reste vide). Réf. : [`packages/skills/thp-video-generation/references/stack-scene-flowchart-layout.md`](../../../packages/skills/thp-video-generation/references/stack-scene-flowchart-layout.md).
  - **Taxonomie texte THP (reproductible)** : limiter le nombre de systèmes en parallèle ; **un type de rendu par rôle** — voir le tableau dans la section suivante. Référence visuelle : [`TextDemo.tsx`](../../../apps/remotion/src/remotion/compositions/demos/TextDemo.tsx).
  - **Catalogue démo & structure THP** : les compositions sous `apps/remotion/src/remotion/compositions/demos/` (`TextDemo`, `DemoShowcaseSolarpunkDemo`, `TransitionsDemo`, `CodeDemo`, `DiagramsDemo`, etc.) constituent la **bibliothèque de patterns** pour une identité reproductible. **Avant** d’inventer une transition ou un effet isolé, vérifier si une démo ou un composant existe déjà (`FadeSlide`, `ZoomBlur`, `Wipe`, etc., avec **`solarTheme`**). Les **nouveaux** blocs « signature » THP (transition, lower-third, bumper) : implémentation sous `packages/ui` / `packages/remotion-lib` si réutilisable, **`theme={solarTheme}`**, puis **enregistrement** d’une mini-composition démo + **ligne dans** [solarpunk-theme-decisions — Catalogue démo & motion](../reference/solarpunk-theme-decisions.md#catalogue-démo--motion-reproductibilité) et mise à jour de ce §04 si ça change la procédure.

### Taxonomie texte THP (reproductible)

**Source unique** : [`packages/skills/thp-video-generation/SKILL.md`](../../../packages/skills/thp-video-generation/SKILL.md) + [`references/library-matrix.md`](../../../packages/skills/thp-video-generation/references/library-matrix.md).

Rappels de gouvernance :

- Le runbook décrit le flux; la matrice rôle -> animation vit dans le skill/matrix.
- `solarTheme`/`theme.ts` définit les tokens visuels, pas le mapping éditorial.
- Global v1: intro hero title par défaut en `GlitchText`; exceptions documentées dans l'outline + §07.

**Règle d’or** : **un rôle -> un type de rendu** par plan (éviter `Typewriter` + `WordByWord` sur la même phrase). Référence live : [`TextDemo.tsx`](../../../apps/remotion/src/remotion/compositions/demos/TextDemo.tsx).

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
  - **Pilot 02 (Git vs GitHub, 2026-03)** : [outline](../video-ai-preparation/pilot-02-git-vs-github-outline.md). Format 1 (45 s) ; **refonte équipe §04** : `TextReveal` / `Typewriter` (`@repo/ui/remotion`), `SceneHeader`, `ProgressBar`, `FadeSlide`, mini `FlowChart` scène 4. Fichiers : `pilot02-content.ts`, `Pilot02GitVsGithub.tsx` ; id **`Pilot02GitVsGithub`**, **1350 f** @ 30 fps.  
  - **Pilot 03 (Commit, 2026-03)** : [outline](../video-ai-preparation/pilot-03-commit-outline.md). Format 1 (45 s) ; même chrome série 01 (`Serie01SceneShell`, `GlitchText` / `TextReveal` / `Typewriter`, transitions variées vs pilot 02), **`FlowChart`** 3 nœuds scène 4 (Modifier → Enregistrer → Commit). Fichiers : `pilot03-content.ts`, `Pilot03Commit.tsx` ; id **`Pilot03Commit`**, **1350 f** @ 30 fps.  
  - **Pilot 04 (Branch, 2026-03)** : [outline](../video-ai-preparation/pilot-04-branch-outline.md). Format 1 (45 s) ; même chrome série 01, **`FlowChart`** 3 nœuds scène 4 (main → Branche → Commits). Fichiers : `pilot04-content.ts`, `Pilot04Branch.tsx` ; id **`Pilot04Branch`**, **1350 f** @ 30 fps.  
  - **Retour 1 (2026-03)** : « Pas assez vivant, pas d’animations, même le terminal n’est pas animé. » → **Actions** : (1) Utiliser le composant [Terminal](packages/ui/src/lib/remotion/code/Terminal.tsx) de `@repo/ui/remotion` (typewriter, ligne par ligne) pour les scènes terminal au lieu de CodeBlockStatic. (2) Renforcer les animations (FadeIn + translateY, entrées plus dynamiques). (3) Référence ton/script : [reference/thp-tone-and-theme](reference/thp-tone-and-theme.md) (analyse cours Intro week_01) pour aligner le ton des vidéos avec THP.
  - **Thème (2026-03-19)** : THP = **Solarpunk dark** comme thème principal ; `:root` web aligné ; `defaultTheme` Remotion = `solarTheme` ; source CSS `solarpunk.tokens.css` ; icônes via `@repo/ui/icons` (Lucide) ; skill Cursor `thp-solarpunk-visual`. Détail et tableau d’amélioration continue : [solarpunk-theme-decisions](../reference/solarpunk-theme-decisions.md).
  - **Retour 2 (2026-03) — fluidité / « trop de temps en plein écran »** : feedback sur le **rythme** (exécution pas assez smooth, sensation d’écran complet statique). **Actions récurrentes** (désormais norme en §04) : (1) découper les scènes en **sous-séquences** avec pattern setup/reveal/hold ; (2) **typewriter** (ou équivalent) sur l’**intro** et textes narratifs longs ; (3) **timings** dans `pilot01-content.ts` (ou équivalent) ; (4) recalibrer **Terminal** (delays + hold sur sortie) ; (5) micro-chevauchements entre éléments. **Références** : [runbooks/remotion](remotion.md#pacing-et-structure-des-scènes) ; démo `TextDemo` ; skill **remotion-best-practices** (`sequencing.md`, `text-animations.md`, `timing.md`).
  - **Retour 3 (2026-03) — V0.6 / lectures scène & identité reproductible** : (1) **Éléments qui disparaissent** avant la fin d’une scène : causé par des sous-`Sequence` trop courtes — corrigé par **durée de montage jusqu’à la fin de la scène parente** (§04 *Sous-Sequence et contenu persistant*). (2) **Chevauchements** : distinguer chevauchement voulu (handoff) vs **collision** (layout) ; ajuster `position` / flex si le texte se superpose. (3) **Trop de systèmes texte** sans rôle : adopter la **taxonomie** §04 (titre = `TextReveal`, narration = `Typewriter`, etc.). (4) **Bases reproductibles THP** : documenter les **démos** comme catalogue de patterns et rattacher **transitions + mouvement** à `solarTheme` / springs du kit — voir [solarpunk-theme-decisions](../reference/solarpunk-theme-decisions.md) § catalogue démo & motion.
  - **Série 01 — Harmonisation §04 + chrome (2026-03-20)** : **Pilot 01** aligné sur **7** segments `SceneHeader` (titre … CTA), **TextReveal** titres/CTA, **Typewriter** avec curseur masqué (`SHOW_TYPEWRITER_CURSOR`), **WordByWord** sur une phrase (recap ligne 2), **FadeSlide** sur les steps/recap ; **Pilot 02** migré sur **`Serie01SceneShell`** (`layout` `center` | `stack` pour scène GitHub + `FlowChart`). Fichiers : `Serie01SceneShell.tsx`, `Pilot01Prerequis.tsx`, `pilot01-content.ts`, `Pilot02GitVsGithub.tsx`. KM : cartographie §04 dans les outlines pilot 01/02, paragraphe série dans [serie-01-git-github](../video-ai-preparation/serie-01-git-github.md).
  - **Série 01 — Consolidation workflow (2026-03-20, cycle long)** : source unique déplacée sur `packages/skills/thp-video-generation` (SKILL + `references/library-matrix.md`), template enrichi (text role mapping + exceptions), et révision 01/02 selon la matrice stricte : **hero intro `GlitchText`**, sous-titre intro `TextReveal`, narration `Typewriter`, emphase ciblée `WordByWord`, CLI `Terminal`. Ajustements concrets : pilot 01 (`TITLE` en glitch, `SUBTITLE` en reveal, ligne OS en `WordByWord` vert), pilot 02 (`TITLE` en glitch, `TITLE_SUBTITLE` en reveal). QA technique : lint/build OK + still renders (`bunx remotion still`) sur frames clés des deux comps ; lecture Studio complète reste une validation manuelle de livraison.
  - **Série 01 — Apprentissage DemoShowcaseSolarpunk (2026-03-20)** : extraction explicite des patterns dynamiques (couches visuelles faibles opacité, cadence de transitions variées `FadeSlide`/`ZoomBlur`/`Wipe`, progression texte par rôle) et application sur les deux pilots. Implémentation: `Serie01SceneShell` supporte désormais `transition` (`fade-slide`, `zoom-blur`, `wipe-left`), et les deux compositions 01/02 ajoutent les overlays diagonaux + vine pattern à faible opacité pour retrouver la signature visuelle sans nuire à la lisibilité pédagogique.
  - **Série 01 — Retour positionnement/persistance (2026-03-20)** : correction d’un bug visuel sur pilot 01 où le terminal/code apparaissait trop bas et certains beats de la scène “OUVRIR” disparaissaient avant le cut. Règle consolidée: (1) les beats pédagogiques critiques restent montés jusqu’à la fin de la scène (`duration = sceneDuration - from`), (2) les blocs terminal/code doivent être positionnés au-dessus du footer/chrome (zone sûre), avec vérification Studio sur les scènes `OUVRIR`, `PWD`, `LS`.

## 08 – Skills utiles au workflow vidéo

Le dossier `packages/skills/` sert de point d’entrée pour les skills utiles à la création vidéo.

- **Submodules** : inclure **`packages/skills/Remotion`** lors du clone (`--recurse-submodules` ou `git submodule update --init packages/skills/Remotion`). Les symlinks **`remotion-best-practices`** (`packages/skills/`, **`.agents/skills/` à la racine**, `apps/remotion/.agents/skills/`) pointent vers `Remotion/skills/remotion`. Détail : [`packages/skills/README.md`](../../../packages/skills/README.md).
- **Préconfiguration Cursor / multi-agent** : exécuter **`bun run bootstrap:agents`** après clone (crée **`.cursor/skills/`** à partir de `packages/skills/`). Les **Background Agents** Cursor peuvent utiliser [`.cursor/environment.json`](../../../.cursor/environment.json) (`install` = submodule Remotion + `bun install` + bootstrap). **Important** : Cursor charge les **Agent Skills** depuis **`.cursor/skills/`**, pas depuis `apps/remotion/.agents/` — ce dernier sert surtout aux agents dont le CWD est l’app Remotion ou d’autres stacks.
- **Cursor (agents) — pas de déclenchement « automatique » garanti** : après bootstrap, charger explicitement les skills utiles si besoin ; pour un pilot Remotion, **chargez** `thp-video-generation` et `thp-solarpunk-visual` (tokens ; ce skill n’est pas sous `packages/skills/` — voir [meta/thp-solarpunk-visual-skill](../meta/thp-solarpunk-visual-skill.md)). Réf. [meta/thp-video-generation-skill](../meta/thp-video-generation-skill.md).
- **THP / Video-AI project skill** : [`packages/skills/thp-video-generation/SKILL.md`](../../../packages/skills/thp-video-generation/SKILL.md) — author pipeline (visual choice by content type, Storybook → Remotion demo → doc update), matrix [`references/library-matrix.md`](../../../packages/skills/thp-video-generation/references/library-matrix.md). Use together with the official Remotion skill below.
- **Remotion (remotion-best-practices)** : submodule **`packages/skills/Remotion`** + symlinks versionnés ; détail [`packages/skills/README.md`](../../../packages/skills/README.md).
- **Diagrammes Mermaid → SVG** : défaut **sans skill** — `bunx @mermaid-js/mermaid-cli` (§03b) ; option **pretty-mermaid** ([skills.sh](https://skills.sh/imxv/pretty-mermaid-skills/pretty-mermaid), voir `packages/skills/README.md`) pour thèmes / batch ; checklist ordre de travail : `diagrams/<slug>/ASSET-PIPELINE.md`.
- **Option génération SVG illustratif par IA** : skill `@neversight/generate-svg` (agentskill.sh) pour logos/illustrations vectorielles à intégrer ensuite dans les scènes.

Voir l’index interne : `packages/skills/README.md`.

### Triggers agent (phrases + rules Remotion)

**Objectif** : forcer le chargement conjoint du **pipeline THP** et des **rules officielles Remotion** (`remotion-best-practices`), pas seulement les skills métier.

1. **Avant la session** : `bun run bootstrap:agents` (`.cursor/skills/` + submodule `packages/skills/Remotion`).
2. **Phrase canonique (copier-coller en tête de chat)** pour toute tâche script / scènes / timings / composants Remotion :

   > Applique les Agent Skills **thp-video-generation**, **thp-solarpunk-visual** et **remotion-best-practices**. Pour le détail bas niveau Remotion, charge les fichiers `rules/*.md` du skill **remotion-best-practices** selon le tableau ci-dessous (chemins sous `packages/skills/remotion-best-practices/rules/` dans le monorepo).

3. **Carte rapide — type de tâche → rules à ouvrir** (en complément du [SKILL.md](../../../packages/skills/remotion-best-practices/SKILL.md) officiel) :

| Besoin | Rules Remotion (extraits) |
|--------|---------------------------|
| Structure des séquences, rythme, ce qui reste à l’écran | `sequencing.md`, `timing.md`, `compositions.md` |
| Texte animé, typewriter, emphase | `text-animations.md`, `animations.md` |
| Cuts, enchaînements visuels | `transitions.md` |
| Audio, voix, sous-titres, légendes | `audio.md`, `voiceover.md`, `subtitles.md`, `display-captions.md` |
| Images, polices, assets | `images.md`, `fonts.md`, `assets.md` |
| 3D / effets | `3d.md` (et démos THP / kit Solarpunk via **thp-video-generation**) |
| Post-trait / médias externes | `ffmpeg.md`, `extract-frames.md`, `trimming.md` (si applicable) |

4. **Templates** : [Templates/pilot-outline](../Templates/pilot-outline.md) et [checklist Solarpunk](../Templates/thp-solarpunk-visual-checklist.md) reprennent ces triggers ; les outlines pilotes (ex. série 01) doivent citer la phrase canonique.

5. **Rappel** : **thp-video-generation** choisit *quoi* montrer (blocs THP, Storybook → démo) ; **remotion-best-practices** rappelle *comment* le framework Remotion attend que ce soit fait au niveau timing, composition et médias.

---

## 09 – Generic platform apps (API + frontend)

Video-AI now includes a generic platform stack to expose rendered videos and prepare future feedback loops:

- `apps/api` — Hono + Bun API (`/health`, `/videos`, `/videos/:slug`)
- `apps/frontend` — Vite + React public catalogue
- `packages/db` — PostgreSQL schema/migrations/seed
- `packages/contracts` — shared Zod contracts between API and frontend

Runbooks:

- [runbooks/api](api.md)
- [runbooks/frontend](frontend.md)
- [runbooks/postgres-local](postgres-local.md)
- [runbooks/deploy-selfhost-api-frontend](deploy-selfhost-api-frontend.md)

### 09b – New video publication workflow (mandatory)

When a new Remotion composition is considered published, update the platform catalogue in this order:

1. **Render readiness**  
   Confirm composition id, final duration/fps, and public render target URL (or live code playback target).

2. **Database reference** (`packages/db`)  
   Add a new `videos`/`video_versions` entry (seed or migration-based data insert), including:
   - `slug`, `title`, `description`
   - `compositionId`
   - `renderUrl` (if using media URL delivery)
   - `docUrl` (pilot outline / KM documentation link)
   - `publishedAt`

3. **API visibility** (`apps/api`)  
   Verify the new entry appears in:
   - `GET /videos`
   - `GET /videos/:slug`

4. **Frontend visibility** (`apps/frontend`)  
   Ensure the new video appears in the catalogue list and opens in detail view.
   If using code playback (Remotion Player), make sure `compositionId` is registered in `sceneRegistry`.

5. **Documentation update**  
   Update corresponding pilot docs and mention publication in the relevant runbook section when needed.

No new video is considered delivered until API + frontend listing is verified.
