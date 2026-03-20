---
title: "Video-AI Monorepo Architecture"
type: documentation
diataxis: reference
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
├── .agents/
│   └── skills/                # remotion-best-practices → packages/skills (agents with CWD = repo root)
├── .cursor/
│   └── environment.json       # Background Agents: install = submodule + bun + bootstrap (versioned)
├── apps/
│   ├── docs/              # Next.js app (docs)
│   ├── web/               # Next.js app (web)
│   ├── storybook/         # Storybook – component library docs
│   └── remotion/          # Remotion Studio – video generation
│       ├── src/
│       │   ├── index.ts         # Registers RemotionRoot from remotion/Root
│       │   ├── index.css
│       │   └── remotion/
│       │       ├── Root.tsx     # Registers all compositions
│       │       └── compositions/
│       │           ├── demos/
│       │           ├── marketing/
│       │           ├── onboarding/
│       │           └── social/
│       ├── remotion.config.ts
│       └── .agents/skills/      # symlink → packages/skills/remotion-best-practices (committed)
├── packages/
│   ├── ui/                 # @repo/ui – static design system
│   │   └── src/
│   │       ├── *.tsx           # Static components (button, card, code)
│   │       ├── *.stories.tsx   # Storybook stories (colocated)
│   │       └── lib/
│   │           └── remotion/   # Current Remotion components (migrate to remotion-lib)
│   │               ├── text/   # Typewriter, WordByWord, etc.
│   │               ├── code/   # CodeBlock, Terminal, DiffView
│   │               ├── audio/  # Spectrum, Waveform, AudioBar
│   │               └── ...
│   ├── remotion-lib/       # @repo/remotion-lib – target for animated components
│   │   └── src/
│   │       ├── primitives/  # Low-level animation building blocks
│   │       ├── blocks/     # Composed animated components
│   │       ├── sections/    # Composable mini-blocks (not Composition)
│   │       └── index.ts
│   ├── eslint-config/      # @repo/eslint-config
│   ├── typescript-config/  # @repo/typescript-config
│   └── skills/
│       ├── Remotion/       # submodule – remotion-dev/skills (official Remotion agent pack)
│       ├── remotion-best-practices/  # symlink → Remotion/skills/remotion
│       └── thp-video-generation/  # THP Video-AI skill (versioned) — SKILL.md + references/
├── KM/
│   ├── Docs/               # submodule – Project documentation
│   │   ├── 00-architecture.md   # ← this file
│   │   ├── 01-index.md         # Index & links
│   │   ├── Templates/          # Obsidian templates
│   │   │   ├── frontmatter-doc.md
│   │   │   ├── frontmatter-runbook.md
│   │   │   ├── frontmatter-adr.md
│   │   │   ├── frontmatter-research-topic.md   # Research topic (one file per tool/subject)
│   │   │   └── pilot-outline.md   # Video-AI pilot outline template
│   │   ├── reference/            # Canonical reference
│   │   │   ├── video-lifecycle.md
│   │   │   └── tools/            # Stable tool reference (promoted from research)
│   │   │       └── README.md
│   │   ├── explanation/
│   │   ├── research/             # Exploratory notes, evaluations, POCs (not canonical)
│   │   │   ├── README.md
│   │   │   ├── openclaw.md
│   │   │   ├── mastra.md
│   │   │   ├── trigger-dev.md
│   │   │   └── inngest.md
│   │   ├── video-ai-preparation/  # Video-AI formats, shortlist, pilot outlines (write before code)
│   │   │   ├── README.md
│   │   │   ├── video-ai-preparation.md   # Single doc: formats, component shortlist, pilot outline
│   │   │   └── pilot-outline.md   # Pointer to Templates/pilot-outline.md
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

Some paths (e.g. `packages/remotion-lib`, `apps/remotion/src/remotion`, `packages/ui` `src/components/`) describe the target architecture; code may be migrated gradually. Before adding Remotion components or compositions, define formats and script in [video-ai-preparation](video-ai-preparation/video-ai-preparation.md); see [reference/video-lifecycle](reference/video-lifecycle.md).

## Research vs reference (tools)

- **research/** — Exploratory notes, evaluations, comparisons, POCs. Status `draft` or `evaluating`; **not canonical**. Do not treat as specification or ADR unless promoted.
- **reference/tools/** — One page per **adopted** tool or stable pattern: what it is, when to use / when to avoid, links to ADR or runbooks. When a tool is decided, create `reference/tools/<outil>.md` (optionally link back to `research/<outil>.md` as "History & evaluation"); do not duplicate the full evaluation.

## UI vs Remotion

Three levels:

- **Static components (design system)** : `packages/ui` — button, card, code, layout, typography; **no** `useCurrentFrame` / Sequence / Remotion timing; used by all apps; documented **only** in Storybook.
- **Remotion animated components** : `packages/remotion-lib` (target) and currently also `packages/ui/src/lib/remotion` — frame-aware primitives (useCurrentFrame, interpolate, Sequence, AbsoluteFill, etc.), wrapping `packages/ui`; reusable across compositions; **not** used in non-Remotion web apps.
- **Compositions** : `apps/remotion/src/remotion` — `<Composition>` using `@repo/remotion-lib` and/or `@repo/ui/remotion`; registered in `Root.tsx`; organized by folders (e.g. `compositions/demos`, `compositions/marketing`).

### Rules

- A component using `useCurrentFrame` must **never** live in `packages/ui` (static surface). It belongs in `packages/remotion-lib` or, during transition, in `packages/ui/src/lib/remotion`.
- Remotion compositions (`<Composition>`) must **never** live in `packages/remotion-lib`; they live in the app (`apps/remotion`).
- Storybook documents **only** `packages/ui` static components. Animated components are used in Remotion Studio, not Storybook.
- From now on, **new** animated components should be created in `packages/remotion-lib`; existing ones in `packages/ui/lib/remotion` remain until migrated (phase 2).

## Repositories (GitHub links)

| Local path                | GitHub URL                                              | Type      |
|---------------------------|---------------------------------------------------------|-----------|
| `KM/Docs`                 | https://github.com/TheHackingProject/Video-KM.git        | submodule |
| `KM/Course/Intro`         | https://github.com/TheHackingProject/course-intro.git   | submodule |
| `KM/Course/Fullstack`     | https://github.com/TheHackingProject/course-fullstack.git | submodule |
| `KM/Course/React`         | https://github.com/TheHackingProject/next-react.git     | submodule |
| `packages/skills/Remotion`| https://github.com/remotion-dev/skills.git              | submodule |

**Remotion agent skills** : submodule `packages/skills/Remotion`, stable path `packages/skills/remotion-best-practices` + `apps/remotion/.agents/skills/remotion-best-practices` (symlinks). See [`packages/skills/README.md`](../../../packages/skills/README.md).

Workspaces include `packages/remotion-lib`. Compositions live under `apps/remotion/src/remotion`.

## Workspaces (Bun)

- `apps/*`, `packages/*` (excluding submodules under `KM/` and `packages/skills/Remotion`)
