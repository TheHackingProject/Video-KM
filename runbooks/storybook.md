---
title: "Storybook Runbook"
type: runbook
diataxis: how-to
status: published
area: development
tags:
  - documentation
  - runbook
  - storybook
  - components
  - ui
created: 2026-03-11
updated: 2026-03-19
related:
  - "[[00-architecture]]"
  - "[[01-index]]"
  - "[[runbooks/monorepo]]"
  - "[[runbooks/bun-biome]]"
---

# Runbook: Storybook

Component library documentation and development environment using Storybook.

## Overview

- **Storybook app**: `apps/storybook/`
- **Stories location**: `packages/ui/src/*.stories.tsx` (colocated with components)
- **Framework**: Next.js + Storybook 10
- **Port**: `http://localhost:6006` (network: `http://<ip>:6006`). **Not** port `3000` — that was the default Next.js page; the `dev` script in `apps/storybook` now starts Storybook (see [apps/storybook/README](../../../apps/storybook/README.md)).
- **Purpose**: Document and develop UI components from `@repo/ui`
- **Root command**: `bun run storybook` (runs `turbo run storybook --filter=storybook`).

## Architecture

Stories are **colocated** with their components (best practice):

```
packages/ui/src/
├── button.tsx           # Component
├── button.stories.tsx   # Story (colocated)
├── card.tsx
├── card.stories.tsx
├── code.tsx
└── code.stories.tsx
```

Storybook config (`apps/storybook/.storybook/main.ts`) points to `packages/ui/src/`:

```ts
stories: [
  "../../../packages/ui/src/**/*.stories.@(js|jsx|mjs|ts|tsx)"
]
```

## Stack

| Tool | Version | Purpose |
|------|---------|---------|
| Storybook | 10.2.17 | Component explorer |
| @storybook/nextjs-vite | 10.2.17 | Next.js framework support |
| @storybook/addon-docs | 10.2.17 | Documentation |
| @storybook/addon-a11y | 10.2.17 | Accessibility testing |
| @storybook/addon-vitest | 10.2.17 | Interaction testing |
| Playwright | 1.58.2 | Browser automation for tests |
| Chromatic | 5.0.1 | Visual regression testing |

## Usage

### Start Storybook

From the monorepo root:

```bash
# Using Turbo
turbo dev --filter=storybook

# Or directly
cd apps/storybook
bun run storybook
```

Storybook starts at `http://localhost:6006` (accessible on network via `--host 0.0.0.0`).

### Build Storybook (static export)

```bash
cd apps/storybook
bun run build-storybook
```

Output: `apps/storybook/storybook-static/`

## Creating Stories

### Add a new component with story

1. Create the component in `packages/ui/src/`:

   ```tsx
   // packages/ui/src/badge.tsx
   export function Badge({ children, variant = "default" }: BadgeProps) {
     return <span className={`badge badge-${variant}`}>{children}</span>;
   }
   ```

2. Create the story next to it:

   ```tsx
   // packages/ui/src/badge.stories.tsx
   import type { Meta, StoryObj } from "@storybook/react";
   import { Badge } from "./badge";

   const meta: Meta<typeof Badge> = {
     title: "UI/Badge",
     component: Badge,
     tags: ["autodocs"],
   };

   export default meta;
   type Story = StoryObj<typeof Badge>;

   export const Default: Story = {
     args: {
       children: "New",
     },
   };
   ```

3. Refresh Storybook — the new component appears automatically.

### Story file conventions

- **Filename**: `<component>.stories.tsx` (same name as component)
- **Location**: Same folder as the component (`packages/ui/src/`)
- **Import**: Use relative import `./component` (not `@repo/ui/component`)

## Testing

### Interaction tests

Use the `play` function in stories:

```tsx
import { within, userEvent, expect } from "storybook/test";

export const ClickTest: Story = {
  play: async ({ canvasElement }) => {
    const canvas = within(canvasElement);
    const button = canvas.getByRole("button");
    await userEvent.click(button);
    await expect(button).toHaveTextContent("Clicked!");
  },
};
```

### Install Playwright (if skipped during setup)

```bash
bunx playwright install chromium --with-deps
```

### Run tests

```bash
cd apps/storybook
bun run test-storybook
```

## Deployment

### Chromatic (recommended)

[Chromatic](https://www.chromatic.com/) provides visual regression testing and hosted Storybook.

```bash
npx chromatic --project-token=<your-token>
```

### Static hosting

Build and deploy `storybook-static/` to any static host (Vercel, Netlify, GitHub Pages).

## Configuration

### Main config

`apps/storybook/.storybook/main.ts`:

- `stories`: points to `packages/ui/src/**/*.stories.*`
- `addons`: enabled addons
- `framework`: `@storybook/nextjs-vite`

### Preview config

`apps/storybook/.storybook/preview.ts`:

- Global decorators
- Parameters (backgrounds, viewport, etc.)
- Theme configuration

## References

- [Storybook – docs](https://storybook.js.org/docs)
- [Storybook for Next.js](https://storybook.js.org/docs/get-started/frameworks/nextjs)
- [Colocated stories](https://storybook.js.org/docs/configure/story-layout#co-located-stories)
- [Chromatic](https://www.chromatic.com/docs/)
