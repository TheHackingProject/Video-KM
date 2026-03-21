---
title: "Runbook – Remotion"
type: runbook
diataxis: how-to
status: published
area: video
tags:
  - runbook
  - remotion
  - video
  - compositions
  - react
created: 2026-03-11
updated: 2026-03-21
related:
  - "[[00-architecture]]"
  - "[[01-index]]"
  - "[[runbooks/monorepo]]"
  - "[[runbooks/storybook]]"
  - "[[runbooks/video-ai-development]]"
---

# Runbook – Remotion

Before creating compositions or new animated components, define formats and script in [video-ai-preparation](../video-ai-preparation/video-ai-preparation.md); see [video-lifecycle](../reference/video-lifecycle.md).

Procedures for using Remotion to create videos with React.

## Overview

Remotion is a framework for creating videos programmatically using React components. Videos are rendered frame-by-frame, with each frame being a React component.

## Agent workflow (Cursor / multi-agent)

Pour tout travail sur des compositions, des timings ou des médias avec un agent IA : suivre **§08** de [video-ai-development](video-ai-development.md#08--skills-utiles-au-workflow-vidéo) — en particulier **Triggers agent** : charger **thp-video-generation**, **thp-solarpunk-visual** et **remotion-best-practices**, puis ouvrir les fichiers pertinents sous `packages/skills/remotion-best-practices/rules/` (`sequencing.md`, `timing.md`, `text-animations.md`, `transitions.md`, etc.). Préparation machine : `bun run bootstrap:agents`.

## Architecture (target)

**Reusable animated components** (target: `packages/remotion-lib`; current usage: `packages/ui/src/lib/remotion`):

- `packages/remotion-lib/src/` — primitives, blocks, sections (target for new animated components).
- `packages/ui/src/lib/remotion/` — current Remotion components (text, code, audio, 3d, ui, diagrams, characters, transitions, hooks); will migrate to remotion-lib over time.

**Remotion app** (`apps/remotion`):

```
apps/remotion/
├── src/
│   ├── index.ts                  # Registers RemotionRoot from remotion/Root
│   ├── index.css
│   └── remotion/
│       ├── Root.tsx               # Registers all compositions
│       └── compositions/
│           ├── demos/             # Demo compositions
│           ├── marketing/
│           ├── onboarding/
│           └── social/
├── remotion.config.ts
├── .agents/skills/               # symlink remotion-best-practices → packages/skills (see packages/skills/README.md)
└── package.json
```

See [00-architecture](00-architecture.md#ui-vs-remotion) for detailed rules.

## Contribution workflow

1. **Static UI component** → create in `packages/ui`, document in Storybook.
2. **Animated component** → create in `packages/remotion-lib` (primitives, blocks, or sections).
3. **Composition** → create in `apps/remotion/src/remotion/compositions/` (e.g. demos, marketing, onboarding, social), register in `remotion/Root.tsx`.
4. New animated components go in `@repo/remotion-lib`, not in `apps/remotion/src/components` or mixed into static UI.

## Where to put what (three-level workflow)

| Level | Where | Notes |
|-------|--------|--------|
| **UI** | `packages/ui` | Document in Storybook. No `useCurrentFrame`. |
| **Animated** | `packages/remotion-lib` | Primitives, blocks, sections. Frame-aware. |
| **Composition** | `apps/remotion/src/remotion/compositions/` | `<Composition>` components; register in `Root.tsx`. |

## Stack

| Tool | Version | Purpose |
|------|---------|---------|
| Remotion | 4.0.x | Video framework |
| @remotion/cli | 4.0.x | CLI for preview/render |
| TailwindCSS | 4.x | Styling (optional) |
| React | 19.x | UI framework |

## Usage

### Development (Remotion Studio)

```bash
cd apps/remotion
bun run dev
```

Opens Remotion Studio at `http://localhost:3001` with live preview and timeline.

### Render a video

```bash
cd apps/remotion

# Render default composition
bunx remotion render

# Render specific composition
bunx remotion render <CompositionId> out/video.mp4

# Render with options
bunx remotion render <CompositionId> out/video.mp4 --codec=h264 --quality=80
```

### Render a still frame

```bash
bunx remotion still <CompositionId> out/frame.png --frame=30
```

## Creating Compositions

### Basic composition

```tsx
// src/MyComposition.tsx
import { AbsoluteFill, useCurrentFrame, useVideoConfig } from "remotion";

export const MyComposition: React.FC = () => {
  const frame = useCurrentFrame();
  const { fps, durationInFrames, width, height } = useVideoConfig();

  return (
    <AbsoluteFill style={{ backgroundColor: "#0f0f0f" }}>
      <h1 style={{ color: "white" }}>Frame: {frame}</h1>
    </AbsoluteFill>
  );
};
```

### Register composition

Place compositions under `src/remotion/compositions/` and register them in `src/remotion/Root.tsx`:

```tsx
// src/remotion/Root.tsx
import { Composition } from "remotion";
import { MyComposition } from "./compositions/demos";

export const RemotionRoot: React.FC = () => {
  return (
    <Composition
      id="MyVideo"
      component={MyComposition}
      durationInFrames={30 * 10}
      fps={30}
      width={1920}
      height={1080}
    />
  );
};
```

## Using the component library

In **compositions**, import frame-aware components from `@repo/remotion-lib` (when available) or `@repo/ui/remotion`. Do not import frame-aware components from static `packages/ui`.

```tsx
import { AbsoluteFill } from "remotion";
import {
  Typewriter,
  CodeBlock,
  FadeSlide,
  Button,
  Card,
} from "@repo/ui/remotion";

export const MyScene: React.FC = () => {
  return (
    <AbsoluteFill>
      <FadeSlide direction="bottom" delay={0}>
        <Card title="Welcome" icon="👋">
          <Typewriter text="Hello, Remotion!" startFrame={15} />
        </Card>
      </FadeSlide>
      <Button text="Click me" startFrame={45} />
    </AbsoluteFill>
  );
};
```

## Key Concepts

### Frame-based animation

All animations are deterministic based on the current frame:

```tsx
const frame = useCurrentFrame();
const opacity = interpolate(frame, [0, 30], [0, 1]);
```

### Spring animations

For natural motion:

```tsx
const scale = spring({
  frame,
  fps,
  config: { damping: 10, stiffness: 100 }
});
```

### Sequences

For timing multiple elements:

```tsx
<Sequence from={0} durationInFrames={60}>
  <IntroScene />
</Sequence>
<Sequence from={60} durationInFrames={120}>
  <MainContent />
</Sequence>
```

### Pacing et structure des scènes

Pour les vidéos pédagogiques THP, éviter les longues plages **sans changement visible** : découper en **beats** (sous-`Sequence` ou `Series`), utiliser **typewriter / reveal** pour le narratif quand le texte doit « prendre le temps » (voir composition démo `TextDemo`), et centraliser **delays / holds** dans un module de contenu (ex. `pilot01-content.ts`). Détail opérationnel et critères de review : [runbooks/video-ai-development](../runbooks/video-ai-development.md) §04 (*Pacing, séquences et texte animé*) et §07 (*Retour 2 — fluidité*). Règles Remotion détaillées : skill **remotion-best-practices** (`sequencing.md`, `text-animations.md`, `timing.md`).

**Contenu persistant** : une sous-`Sequence` trop courte **démonte** ses enfants à sa fin — un texte ou un calque peut **disparaître avant le cut** de la grande scène. Si le calque doit rester visible jusqu’à la fin de la scène parente, utiliser `durationInFrames = duréeParente - from` (voir §04 *Sous-Sequence et contenu persistant*).

### Catalogue de démos THP Remotion

Les compositions dans `apps/remotion/src/remotion/compositions/demos/` servent de **référence reproductible** pour l’identité THP :

| Démo | Intérêt |
|------|---------|
| `TextDemo` | `Typewriter`, `WordByWord`, `TextReveal` — base de la [taxonomie texte](../runbooks/video-ai-development.md#taxonomie-texte-thp-reproductible) |
| `DemoShowcaseSolarpunkDemo` | Kit Solarpunk : palette, `Terminal`, timings |
| `TransitionsDemo` | `FadeSlide` et transitions exportées par `@repo/ui/remotion` |
| `CodeDemo`, `DiagramsDemo`, `UIDemo`, … | Patterns à réutiliser avant d’ajouter un one-off ; `DiagramsDemo` inclut **SchematicFlowChart** (texte + Lucide + flèches) — Storybook : `SchematicFlowChartView` |

Nouveaux effets **signature** : préférer les transitions et primitives déjà exposées (`FadeSlide`, `ZoomBlur`, `Wipe`) avec **`theme={solarTheme}`** ; ajouter une démo courte + consigner la décision dans [solarpunk-theme-decisions](../reference/solarpunk-theme-decisions.md#catalogue-démo--motion-reproductibilité).

## Configuration

### remotion.config.ts

```ts
import { Config } from "@remotion/cli/config";

Config.setVideoImageFormat("jpeg");
Config.setOverwriteOutput(true);
Config.setConcurrency(4);
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "You are running Remotion with Bun" warning | Informational only, Bun is supported |
| Components not animating | Ensure using `useCurrentFrame()` |
| Slow renders | Reduce concurrency or resolution |
| Import errors from @repo/ui/remotion | Run `bun install` at monorepo root |

### Fond (background) absent ou « non intégré » au fichier exporté

**À distinguer :** prévisualisation Studio vs fichier final (MP4, ProRes, WebM, PNG).

1. **Rendu avec transparence (alpha)**  
   Si la commande ou la config utilise un **codec / pixel format à canal alpha** (ex. ProRes 4444, VP9 WebM avec `yuva…`, ou séquence PNG avec alpha), les zones non couvertes par des pixels opaques restent **transparentes** : dans un lecteur ou un NLE, le fond peut apparaître noir, gris ou absent — ce n’est pas un bug du gradient React, c’est le comportement du format.  
   Réf. skill (repo) : `packages/skills/Remotion/skills/remotion/rules/transparent-videos.md` — pour un fond **toujours visible** dans un fichier « classique », utiliser **H.264 / pixel format sans alpha** (ex. `yuv420p`) et ne pas forcer `--image-format=png` + codec alpha sauf besoin explicite.

2. **`background-image` chargée via `url(...)`**  
   Remotion ne peut pas attendre fin de chargement pour une image référencée uniquement en CSS (`background-image`, `mask-image`), ce qui peut provoquer **clignotement ou calque vide** au rendu. Préférer le composant `<Img>` de Remotion ou le contournement décrit dans la doc officielle.  
   Réf. : [Troubleshooting – background-image](https://www.remotion.dev/docs/troubleshooting/background-image).

3. **Fond défini dans le composant**  
   Le runbook montre un fond **opaque** via `AbsoluteFill` + `backgroundColor` (fiable pour le pipeline d’encodage). Les **dégradés CSS** inline sur `AbsoluteFill` (comme sur les pilots série 01) sont en général rendus correctement par Chromium ; si un cas limite apparaît, tester `backgroundColor` (couleur de repli) + `backgroundImage` avec le même gradient.

4. **Config projet (`apps/remotion/remotion.config.ts`)**  
   `Config.setVideoImageFormat("jpeg")` définit le format des **frames intermédiaires** vers la vidéo : **pas de canal alpha** ; le fond dessiné par React doit normalement être présent. En cas de souci de qualité / bandes, tester `png` ou ajuster codec / qualité (voir [Remotion – config](https://www.remotion.dev/docs/config)).

## Resources

- [Remotion docs](https://www.remotion.dev/docs)
- [Remotion prompts](https://www.remotion.dev/prompts)
- [Remotion licensing](https://remotion.pro/license) – Free for teams ≤3
