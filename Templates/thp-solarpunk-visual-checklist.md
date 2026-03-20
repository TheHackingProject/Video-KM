---
title: "Checklist visuel THP Solarpunk (template)"
type: documentation
status: published
area: video-ai
tags:
  - template
  - thp
  - solarpunk
  - qa
created: 2026-03-19
updated: 2026-03-20
related:
  - "[[reference/solarpunk-theme-decisions]]"
  - "[[runbooks/video-ai-development]]"
  - "[[meta/thp-video-generation-skill]]"
  - "[[meta/thp-solarpunk-visual-skill]]"
---

# Checklist visuel THP Solarpunk

À utiliser **avant merge** d’une composition Remotion, d’un écran app, ou d’un lot de composants animés. Décisions normatives : [solarpunk-theme-decisions](../reference/solarpunk-theme-decisions.md).

## Session agent (Cursor) — à faire quand c’est pertinent

Pour que la charte **et les rules Remotion** remontent dans le contexte de l’agent et soient **exécutées** (pas seulement lues une fois) :

- [ ] **`bun run bootstrap:agents`** exécuté depuis la racine Video-AI si `.cursor/skills/` incomplet ou submodule Remotion non initialisé — [runbook §08](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo).
- [ ] **`thp-solarpunk-visual`** : présent sous `.cursor/skills/thp-solarpunk-visual/` (symlink ou copie depuis [meta/thp-solarpunk-visual-skill](../meta/thp-solarpunk-visual-skill.md)).
- [ ] **`thp-video-generation`** : présent sous `.cursor/skills/thp-video-generation/` — [meta/thp-video-generation-skill](../meta/thp-video-generation-skill.md).
- [ ] **`remotion-best-practices`** : présent sous `.cursor/skills/remotion-best-practices/` (créé par le bootstrap ; source `packages/skills/remotion-best-practices`).
- [ ] **Phrase type en tête de session** : coller la phrase canonique **§08 — Triggers agent** ([runbook](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo)) incluant les **trois** skills + l’ouverture des **rules** `packages/skills/remotion-best-practices/rules/*.md` selon la tâche (timing, texte, transitions, audio, etc.).

## Palette et tokens

- [ ] Couleurs sémantiques alignées avec `packages/theme/solarpunk.tokens.css` et `solarTheme` (pas de one-off hors charte sans mise à jour du doc décisions).
- [ ] **`error` / corail** réservé aux états d’erreur — pas d’accent décoratif.

## Accessibilité

- [ ] Contrastes **WCAG** vérifiés (texte principal, `textMuted`, blocs code, titres sur fond dégradé).
- [ ] Tailles de texte minimales OK en 1080p export (ou résolution cible du cours).

## Motion et kit

- [ ] Mouvement **organique** (éviter le glitch agressif type cyberpunk sauf intention documentée).
- [ ] Réutilisation des **patterns partagés** (gradients, opacité particules, `SceneHeader`, springs) cohérents avec les autres vidéos / `DemoShowcase`.

## Iconographie

- [ ] **Par défaut** : `@repo/ui/icons` (Lucide) avec couleurs du thème ; SVG maison uniquement si pictogramme signature documenté.

## Documentation

- [ ] Retours notables ajoutés au tableau **Amélioration continue** dans [solarpunk-theme-decisions](../reference/solarpunk-theme-decisions.md) si la charte évolue.
