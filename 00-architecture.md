---
title: "Video-AI Monorepo Architecture"
type: documentation
status: published
area: architecture
tags:
  - documentation
  - architecture
  - monorepo
  - turborepo
  - submodules
created: 2026-03-10
updated: 2026-03-11
related:
  - "[[01-index]]"
  - "[[runbooks/monorepo]]"
  - "[[runbooks/dependencies-submodules]]"
  - "[[runbooks/storybook]]"
  - "[[runbooks/remotion]]"
---

# 00 – Architecture Video-AI

Overview of the monorepo architecture, structure and repository links.

## Directory Tree

```
Video-AI/
├── apps/
│   ├── docs/              # Next.js app (docs)
│   ├── web/               # Next.js app (web)
│   ├── storybook/         # Storybook – component library docs
│   └── remotion/          # Remotion Studio – video generation
│       ├── src/
│       │   ├── Root.tsx        # Remotion entry point
│       │   └── Composition.tsx # Video compositions
│       ├── remotion.config.ts  # Remotion configuration
│       └── skills/             # Agent skills (symlinked)
├── packages/
│   ├── ui/                 # @repo/ui – shared React components
│   │   └── src/
│   │       ├── *.tsx           # Components
│   │       ├── *.stories.tsx   # Storybook stories (colocated)
│   │       └── lib/
│   │           └── remotion/   # Remotion component library
│   │               ├── text/       # Typewriter, WordByWord, etc.
│   │               ├── code/       # CodeBlock, Terminal, DiffView
│   │               ├── audio/      # Spectrum, Waveform, AudioBar
│   │               ├── 3d/         # RotatingObject, FloatingText
│   │               ├── ui/         # Button, Card, Badge (animated)
│   │               ├── diagrams/   # FlowChart, Tree, Timeline
│   │               ├── characters/ # Avatar, SpeakingHead
│   │               ├── transitions/# FadeSlide, ZoomBlur, Wipe
│   │               └── hooks/      # useTypewriter, useSpringAnimation
│   ├── eslint-config/      # @repo/eslint-config
│   ├── typescript-config/  # @repo/typescript-config
│   └── skills/
│       └── Remotion/       # submodule – Remotion Agent Skills
├── KM/
│   ├── Docs/               # submodule – Project documentation
│   │   ├── 00-architecture.md   # ← this file
│   │   ├── 01-index.md         # Index & links
│   │   ├── Templates/          # Obsidian templates
│   │   │   ├── frontmatter-doc.md
│   │   │   ├── frontmatter-runbook.md
│   │   │   └── frontmatter-adr.md
│   │   └── runbooks/
│   │       ├── monorepo.md
│   │       ├── dependencies-submodules.md
│   │       ├── bun-biome.md
│   │       └── storybook.md
│   └── Course/
│       ├── Intro/          # submodule
│       ├── Fullstack/      # submodule
│       └── React/          # submodule
├── turbo.json
├── package.json
└── .gitmodules
```

## Repositories (GitHub links)

| Local path                | GitHub URL                                              | Type      |
|---------------------------|---------------------------------------------------------|-----------|
| `KM/Docs`                 | https://github.com/TheHackingProject/Video-KM.git        | submodule |
| `KM/Course/Intro`         | https://github.com/TheHackingProject/course-intro.git   | submodule |
| `KM/Course/Fullstack`     | https://github.com/TheHackingProject/course-fullstack.git | submodule |
| `KM/Course/React`         | https://github.com/TheHackingProject/next-react.git     | submodule |
| `packages/skills/Remotion`| https://github.com/remotion-dev/skills.git              | submodule |

## Workspaces (Bun)

- `apps/*`, `packages/*` (excluding submodules under `KM/` and `packages/skills/Remotion`)
