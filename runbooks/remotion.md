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
updated: 2026-03-11
related:
  - "[[00-architecture]]"
  - "[[01-index]]"
  - "[[runbooks/monorepo]]"
  - "[[runbooks/storybook]]"
---

# Runbook – Remotion

Procedures for using Remotion to create videos with React.

## Overview

Remotion is a framework for creating videos programmatically using React components. Videos are rendered frame-by-frame, with each frame being a React component.

## Architecture

```
apps/remotion/                    # Remotion application
├── src/
│   ├── Root.tsx                  # Entry point (registers compositions)
│   └── Composition.tsx           # Video compositions
├── remotion.config.ts            # Remotion configuration
├── skills/                       # Agent skills (remotion-best-practices)
└── package.json

packages/ui/src/lib/remotion/     # Reusable Remotion components
├── text/                         # Typewriter, WordByWord, TextReveal, GlitchText
├── code/                         # CodeBlock, Terminal, DiffView
├── audio/                        # Spectrum, Waveform, AudioBar
├── 3d/                           # RotatingObject, FloatingText, ParticleField
├── ui/                           # Button, Card, Badge, ProgressBar, SceneHeader
├── diagrams/                     # FlowChart, Tree, Timeline, ComparisonTable
├── characters/                   # Avatar, SpeakingHead, Silhouette
├── transitions/                  # FadeSlide, ZoomBlur, Wipe
├── hooks/                        # useTypewriter, useSpringAnimation, useAudioData
├── theme.ts                      # Default theme configuration
└── index.ts                      # Exports all components
```

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

```tsx
// src/Root.tsx
import { Composition } from "remotion";
import { MyComposition } from "./MyComposition";

export const RemotionRoot: React.FC = () => {
  return (
    <>
      <Composition
        id="MyVideo"
        component={MyComposition}
        durationInFrames={30 * 10} // 10 seconds at 30fps
        fps={30}
        width={1920}
        height={1080}
      />
    </>
  );
};
```

## Using the Component Library

Import components from `@repo/ui/remotion`:

```tsx
import { 
  Typewriter, 
  CodeBlock, 
  FadeSlide,
  Button,
  Card 
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

## Resources

- [Remotion docs](https://www.remotion.dev/docs)
- [Remotion prompts](https://www.remotion.dev/prompts)
- [Remotion licensing](https://remotion.pro/license) – Free for teams ≤3
