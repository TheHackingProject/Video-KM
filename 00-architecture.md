---
title: "Video-AI Monorepo Architecture"
type: documentation
status: published
area: architecture
tags:
  - "#documentation"
  - "#architecture"
  - "#monorepo"
  - "#turborepo"
  - "#submodules"
created: "2026-03-10"
updated: "2026-03-10"
related:
  - "01-index.md"
  - "runbooks/monorepo.md"
  - "runbooks/dependencies-submodules.md"
---

# 00 – Architecture Video-AI

Overview of the monorepo architecture, structure and repository links.

## Directory Tree

```
Video-AI/
├── apps/
│   ├── docs/              # Next.js app (docs)
│   └── web/                # Next.js app (web)
├── packages/
│   ├── ui/                 # @repo/ui – shared React components
│   ├── eslint-config/      # @repo/eslint-config
│   ├── typescript-config/  # @repo/typescript-config
│   └── skills/
│       └── Remotion/       # submodule – Remotion Agent Skills
├── KM/
│   ├── Docs/               # submodule – Project documentation
│   │   ├── 00-architecture.md   # ← this file
│   │   ├── 01-index.md         # Index & links
│   │   └── runbooks/
│   │       ├── monorepo.md
│   │       ├── dependencies-submodules.md
│   │       └── bun-biome.md
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
