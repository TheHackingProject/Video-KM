---
title: "Bun and Biome Runbook"
type: runbook
status: published
area: development
tags:
  - documentation
  - runbook
  - bun
  - biome
  - lint
  - format
created: 2026-03-10
updated: 2026-03-11
related:
  - "[[00-architecture]]"
  - "[[01-index]]"
  - "[[runbooks/monorepo]]"
---

# Runbook: Bun and Biome

Installation and usage of Bun (package manager) and Biome (lint + format) in Video-AI.

---

## Bun (package manager)

### Role

- **Package manager**: install dependencies, run scripts, manage workspaces (like npm/pnpm/yarn).
- Declared in the repo: `"packageManager": "bun@1.3.10"` in the root `package.json`.

### Local installation

```bash
# Via curl (see https://bun.sh)
curl -fsSL https://bun.sh/install | bash
```

### Usage in Video-AI

From the monorepo root:

```bash
bun install          # Install dependencies
bun run build        # Run "build" script
bun run dev          # Start dev
bun add -d <pkg>     # Add a devDependency at root
```

Workspaces (`apps/*`, `packages/*`) are managed by Bun as a standard monorepo.

---

## Biome (lint + format)

### Role

- **Linter** and **formatter** (replaces or complements ESLint + Prettier).
- Config in `biome.json` at the root of the Video-AI repo.
- Version used: `@biomejs/biome` as devDependency (e.g. ^2.4.6).

### Project installation

Already installed as devDependency at root:

```bash
bun add -d @biomejs/biome
```

Create or update config:

```bash
bunx @biomejs/biome init
```

### Important: which command to run

On some systems, the **`biome`** terminal command points to **another tool** (e.g. `biom` from the `python3-biom-format` package), not the Biome linter. You may see:

```text
Command 'biome' not found, did you mean: command 'biom' from deb python3-biom-format
```

**Use in Video-AI** (from the repo root):

```bash
bunx biome check          # Check format + lint (no edits)
bunx biome check --write  # Format + lint + auto-fixes
bunx biome lint --write   # Lint + fixes
bunx biome format --write # Format only
```

Or with npx if you don't use Bun for the CLI:

```bash
npx @biomejs/biome check --write
```

**Avoid**: relying on an unqualified global `biome` command, unless you have explicitly installed the **@biomejs/biome** binary globally and confirmed it is in your `PATH`.

### Useful commands

| Purpose           | Command                          |
|-------------------|----------------------------------|
| Check everything  | `bunx biome check`               |
| Fix + format      | `bunx biome check --write`       |
| Lint only         | `bunx biome lint` / `bunx biome lint --write` |
| Format only       | `bunx biome format --write`      |
| CI (no write)     | `bunx biome ci`                  |

### Current config (excerpt)

- **VCS**: Git, useIgnoreFile
- **Formatter**: enabled, indent = tab
- **Linter**: enabled, rules recommended
- **JS**: quoteStyle double
- **Assist**: organizeImports on

Full config: `biome.json` at the root of Video-AI.

### Editor (IntelliJ / VS Code)

- **IntelliJ**: official "Biome" plugin (Settings → Plugins). Uses the project's `biome.json`.
- **VS Code**: official Biome extension. Disable ESLint/Prettier for this repo to avoid conflicts.

### Migration from ESLint / Prettier

- Official guide: [Biome – Migrate from ESLint and Prettier](https://biomejs.dev/guides/migrate-eslint-prettier)
- The `biome migrate eslint` and `biome migrate prettier` commands belong to the **Biome CLI** (`@biomejs/biome`). If the `biome` terminal command points to the other tool (biom), use explicitly:

  ```bash
  bunx @biomejs/biome migrate eslint
  bunx @biomejs/biome migrate prettier
  ```

---

## Summary

| Tool  | Role                     | Typical command (from root)                |
|-------|--------------------------|--------------------------------------------|
| Bun   | Install, scripts, workspaces | `bun install`, `bun run build`          |
| Biome | Lint + format            | `bunx biome check --write`, `bunx biome ci` |

Always use **`bunx biome`** (or **`npx @biomejs/biome`**) for the Biome linter/formatter, not the unqualified global `biome` command.
