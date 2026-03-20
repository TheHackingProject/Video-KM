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
updated: 2026-03-19
related:
  - "[[reference/solarpunk-theme-decisions]]"
  - "[[runbooks/video-ai-development]]"
---

# Checklist visuel THP Solarpunk

À utiliser **avant merge** d’une composition Remotion, d’un écran app, ou d’un lot de composants animés. Décisions normatives : [solarpunk-theme-decisions](../reference/solarpunk-theme-decisions.md).

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
