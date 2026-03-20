---
title: "Skill agent — THP Solarpunk visual (contenu versionné)"
type: documentation
area: meta
status: published
tags:
  - cursor
  - skill
  - solarpunk
created: 2026-03-19
updated: 2026-03-19
related:
  - "[[reference/solarpunk-theme-decisions]]"
---

# Skill agent : THP Solarpunk visual

Le dépôt ignore `.cursor/` dans `.gitignore` : ce fichier est la **copie versionnée** du skill. Pour Cursor, tu peux recopier le contenu vers `.cursor/skills/thp-solarpunk-visual/SKILL.md` en local ou suivre ce document manuellement.

## When to use

Lors de l’implémentation ou la revue **UI / Remotion / Storybook** THP — charte Solarpunk dark.

## Source of truth

1. [solarpunk-theme-decisions](../reference/solarpunk-theme-decisions.md)
2. [solarpunk-theme-reflection](../reference/solarpunk-theme-reflection.md)
3. `packages/theme/solarpunk.tokens.css`
4. `packages/ui/src/lib/remotion/theme.ts` (`solarTheme`, `defaultTheme === solarTheme`)
5. `packages/ui/src/lib/remotion/demo-showcase/config.ts`
6. **Icônes UI** : `@repo/ui/icons` (`lucide-react`)
7. `packages/ui/src/assets/thp/svg/` — réservé futurs SVG signature (vide par défaut)

## Rules (summary)

- Dark only ; `--error` réservé aux erreurs ; WCAG prioritaire ; motion organique ; SVG THP préférés ; ton [thp-tone-and-theme](../reference/thp-tone-and-theme.md).

## Templates

- [thp-solarpunk-visual-checklist](../Templates/thp-solarpunk-visual-checklist.md)
- [pilot-outline](../Templates/pilot-outline.md)
