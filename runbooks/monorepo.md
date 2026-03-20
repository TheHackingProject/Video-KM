---
title: "Monorepo (Turborepo) Runbook"
type: runbook
diataxis: how-to
status: published
area: development
tags:
  - documentation
  - runbook
  - monorepo
  - turborepo
  - bun
created: 2026-03-10
updated: 2026-03-11
related:
  - "[[00-architecture]]"
  - "[[01-index]]"
  - "[[runbooks/dependencies-submodules]]"
  - "[[runbooks/bun-biome]]"
  - "[[runbooks/storybook]]"
---

# Runbook: Monorepo (Turborepo)

Installation and usage summary for the Video-AI monorepo with Turborepo.

## Stack

- **Turborepo**: task orchestration (build, lint, dev) with caching and parallelism
- **Package manager**: Bun
- **Workspaces**: `apps/*`, `packages/*`

## Structure

```
Video-AI/
├── apps/
│   ├── docs/       # Next.js app (docs)
│   └── web/        # Next.js app (web)
├── packages/
│   ├── ui/                 # @repo/ui – shared React components
│   ├── eslint-config/      # @repo/eslint-config
│   └── typescript-config/  # @repo/typescript-config
├── turbo.json
├── package.json
└── ...
```

## Installation (fresh clone)

From the repo root:

```bash
# Clone with submodules (KM/Docs, KM/Course/*)
git clone --recurse-submodules <repo-url>
cd Video-AI

# Install dependencies (Bun)
bun install
```

If the repo was already cloned without submodules:

```bash
git submodule update --init --recursive
bun install
```

(Submodule init must include `packages/skills/Remotion` for **remotion-best-practices** symlinks to resolve; see [`packages/skills/README.md`](../../../packages/skills/README.md).)

## Root scripts

| Script      | Command               | Purpose                                  |
|-------------|------------------------|------------------------------------------|
| Build       | `bun run build`        | Build all apps/packages (Turbo)          |
| Dev         | `bun run dev`          | Start dev servers (Turbo)                 |
| Lint        | `bun run lint`        | Lint all packages (Turbo)                 |
| Check-types | `bun run check-types`  | TypeScript check (Turbo)                  |
| Format      | `bun run format`       | Prettier (migrate to Biome if needed)    |

## Common usage

- **Build everything**: `bun run build` or `turbo build`
- **Develop one app**: `turbo dev --filter=docs` or `turbo dev --filter=web`
- **Build one package**: `turbo build --filter=docs`
- **Run all in dev**: `turbo dev`

## Turbo configuration (`turbo.json`)

- `build`: depends on `^build` (build dependencies first), outputs `.next/**`
- `lint`: depends on `^lint`
- `check-types`: depends on `^check-types`
- `dev`: `persistent: true`, `cache: false`

Tasks run in dependency graph order and are cached when inputs are unchanged.

## References

- [Turborepo – Getting started](https://turborepo.dev/docs/getting-started)
- [Turborepo – Add to existing repository](https://turborepo.dev/docs/getting-started/add-to-existing-repository)
