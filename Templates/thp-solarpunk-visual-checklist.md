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
updated: 2026-03-23
related:
  - "[[reference/solarpunk-theme-decisions]]"
  - "[[runbooks/video-ai-development]]"
  - "[[meta/thp-video-generation-skill]]"
  - "[[meta/thp-solarpunk-visual-skill]]"
---

# Checklist visuel THP Solarpunk

À utiliser **avant merge** d’une composition Remotion, d’un écran app, ou d’un lot de composants animés. Décisions normatives : [solarpunk-theme-decisions](../reference/solarpunk-theme-decisions.md).

## Session agent (Cursor) — à faire quand c’est pertinent

Pour que la charte **remonte** dans le contexte de l’agent et soit **exécutée** (pas seulement lue une fois) :

- [ ] **`thp-solarpunk-visual`** : installé sous `.cursor/skills/thp-solarpunk-visual/` depuis [meta/thp-solarpunk-visual-skill](../meta/thp-solarpunk-visual-skill.md) (symlink ou copie).
- [ ] **`thp-video-generation`** : installé sous `.cursor/skills/thp-video-generation/` depuis `packages/skills/thp-video-generation/` — [meta/thp-video-generation-skill](../meta/thp-video-generation-skill.md).
- [ ] **Chargement explicite** en début de session sur la composition : mentionner les deux skills dans le chat (Cursor ne garantit pas l’auto-sélection) — détail [runbook §08](../runbooks/video-ai-development.md#08--skills-utiles-au-workflow-vidéo).

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
