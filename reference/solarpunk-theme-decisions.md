---
title: "Décisions thème THP Solarpunk — source de vérité et amélioration continue"
type: documentation
diataxis: reference
status: published
area: design
tags:
  - solarpunk
  - thp
  - theme
  - accessibility
  - video-ai
created: 2026-03-19
updated: 2026-03-19
related:
  - "[[reference/solarpunk-theme-reflection]]"
  - "[[reference/thp-tone-and-theme]]"
  - "[[runbooks/video-ai-development]]"
  - "[[design/brief-icones-thp-pour-designer]]"
---

# Décisions thème THP Solarpunk

Document **normatif** : décisions validées par l’équipe / produit. Pour le contexte et la recherche, voir [solarpunk-theme-reflection](solarpunk-theme-reflection.md).

---

## Décisions (2026-03-19)

| # | Sujet | Décision |
|---|--------|----------|
| 1 | Identité | **Solarpunk = thème principal** Video-AI / THP (pas une variante optionnelle). |
| 2 | Mode | **Dark only** — pas de thème clair Solarpunk pour l’instant. |
| 3 | Web `:root` | **Solarpunk réel par défaut** : les tokens sémantiques du site pointent sur la palette Solarpunk sans exiger `data-theme` (l’attribut reste accepté, mêmes valeurs). Fichier : [`packages/theme/solarpunk.tokens.css`](../../../packages/theme/solarpunk.tokens.css). |
| 4 | Source de vérité | **Oui** : `solarpunk.tokens.css` (web) + `solarTheme` dans [`theme.ts`](../../../packages/ui/src/lib/remotion/theme.ts) — **mêmes hex sémantiques** ; palette étendue showcase dans [`demo-showcase/config.ts`](../../../packages/ui/src/lib/remotion/demo-showcase/config.ts) documentée comme dérivée. Procédure et liens : ce fichier + [packages/theme/README](../../../packages/theme/README.md). |
| 5 | `--error` / corail | **Réservé aux états d’erreur et alertes** — ne pas l’utiliser comme accent décoratif (lisibilité + sémantique). |
| 6 | Accessibilité | **Très important** : vérifier **contrastes WCAG** (texte / fond, code, `textMuted`) à chaque évolution de palette ou nouvelle composition ; corriger les paires insuffisantes en priorité. |
| 7 | Typographie | **Direction Solarpunk pour tout** (titres + corps + code dans le système Remotion) : stack actuelle Poppins / Inter / JetBrains Mono ; évolution possible vers une display plus « organique » (ex. Brassia) si licence et lisibilité OK — documenter le changement ici. |
| 8 | Vidéos THP | **THP = Solarpunk** : toutes les vidéos cours / démos suivent le kit visuel (couleurs, mouvement organique, composants). |
| 9 | Kit visuel | **Oui** : réutiliser les **mêmes** patterns (dégradés, `ParticleField`, rayons, `SceneHeader`, timings springs) sur **tous** les composants et effets animés — éviter les one-off hors charte. |
| 10 | Iconographie | **Par défaut : Lucide** (`@repo/ui/icons`). **SVG signature** THP uniquement si commande design — dépôt vide réservé : [`assets/thp/svg/README`](../../../packages/ui/src/assets/thp/svg/README.md). Emoji possibles si le script l’exige. |

---

## Implémentation technique (rappel)

| Zone | Fichier / entrée |
|------|------------------|
| Tokens CSS | `packages/theme/solarpunk.tokens.css` importé par `packages/theme/theme.css` |
| Remotion | `export const defaultTheme = solarTheme` dans `packages/ui/src/lib/remotion/theme.ts` |
| Showcase étendu | `packages/ui/src/lib/remotion/demo-showcase/config.ts` (`COLORS`) — aligné conceptuellement, pas forcément 1:1 avec les 7 variables CSS |
| Icônes THP | `@repo/ui/icons` (`lucide-react`, `thp-lucide.ts`) — Storybook **THP / Lucide icons** |

---

## Amélioration continue — retours à consigner

À **compléter après chaque pilot, revue design ou audit accessibilité**. Copier une ligne modèle dans le tableau.

| Date | Source | Retour / observation | Action / décision de suivi |
|------|--------|----------------------|----------------------------|
| 2026-03-19 | Atelier thème | Validation des 10 décisions ci-dessus ; `:root` aligné Solarpunk ; `defaultTheme` = `solarTheme`. | Poursuivre avec SVG THP + audit contrastes sur `textMuted` / code. |
| 2026-03-19 | Lot 1 icônes | Abandonné : SVG placeholder supprimés ; **Lucide** + `@repo/ui/icons`. | Utiliser `thp-lucide.ts` dans les écrans. |
| 2026-03-19 | Choix produit | **Icônes UI = Lucide** (`lucide-react`) + export `@repo/ui/icons` ; SVG maison = optionnel / legacy. | Utiliser Lucide dans les nouveaux écrans ; réserver le brief designer aux pictos signature. |
| | | | |

**Process** : en cas de changement de procédure ou de palette, mettre à jour aussi [runbook Video-AI Development §07](../runbooks/video-ai-development.md#07--amélioration-du-process) et les templates [thp-solarpunk-visual-checklist](../Templates/thp-solarpunk-visual-checklist.md) / [pilot-outline](../Templates/pilot-outline.md).

---

## Skill agent (Cursor)

Contenu versionné : [meta/thp-solarpunk-visual-skill](../meta/thp-solarpunk-visual-skill.md) (à recopier en local vers `.cursor/skills/thp-solarpunk-visual/SKILL.md` si besoin — le dossier `.cursor/` est souvent gitignoré).
