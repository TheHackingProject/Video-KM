---
title: "Réflexion thème Solarpunk — recherche et repères visuels"
type: documentation
diataxis: reference
status: published
area: design
tags:
  - solarpunk
  - theme
  - remotion
  - ui
  - video-ai
created: 2026-03-19
updated: 2026-03-19
related:
  - "[[reference/solarpunk-theme-decisions]]"
  - "[[reference/thp-tone-and-theme]]"
  - "[[video-ai-preparation/video-ai-preparation]]"
---

# Réflexion thème Solarpunk

Document de **recherche** et **repères visuels** (mouvement, histoire du genre). Les **décisions produit** et la **boucle d’amélioration continue** sont dans **[solarpunk-theme-decisions](solarpunk-theme-decisions.md)** (source normative).

**Implémentation dans le repo**

- Tokens web : [`packages/theme/solarpunk.tokens.css`](../../../packages/theme/solarpunk.tokens.css) (importé par `theme.css`).
- Thème Remotion : [`packages/ui/src/lib/remotion/theme.ts`](../../../packages/ui/src/lib/remotion/theme.ts) — `solarTheme` ; `defaultTheme` pointe sur le même objet.
- Palette étendue démo : [`packages/ui/src/lib/remotion/demo-showcase/config.ts`](../../../packages/ui/src/lib/remotion/demo-showcase/config.ts) (`COLORS`).
- Icônes UI : `@repo/ui/icons` ([`thp-lucide.ts`](../../../packages/ui/src/thp-lucide.ts)).

**Skill agent (Cursor)** : [meta/thp-solarpunk-visual-skill](../meta/thp-solarpunk-visual-skill.md) (versionné ; copie locale possible sous `.cursor/skills/`)

---

## Ce qu’est le solarpunk (repères)

Mouvement **littéraire, artistique et social** qui imagine des futurs **durables et optimistes**, où **technologie et nature coexistent** (contrasté avec le cyberpunk dystopique). Le « solar » évoque les **énergies renouvelables** et un imaginaire **constructif** ; le « punk » garde une dimension **DIY**, **communautaire** et parfois **contre-culturelle**. Sources de synthèse : [Wikipedia — Solarpunk](https://en.wikipedia.org/wiki/Solarpunk), [One Earth — Solarpunk](https://www.oneearth.org/solarpunk/), [Aeon — villes solarpunk](https://aeon.co/essays/in-solarpunk-cities-of-the-future-tech-follows-natures-lead).

Pour le **design**, on retient souvent : **lignes organiques** (héritage [Art nouveau](https://en.wikipedia.org/wiki/Art_Nouveau)), **végétation intégrée** à l’urbain, **lumière du jour**, **matériaux naturels** ou recyclés, **symboles d’énergie renouvelable**, compositions **centrées communauté** et **espoir** plutôt que collapse. Voir par ex. [Sound of Life — aperçu esthétique](https://www.soundoflife.com/blogs/design/solarpunk-aesthetic), [Number Analytics — guide design](https://www.numberanalytics.com/blog/solarpunk-design-guide), [Medium — Verdant Circuit](https://medium.com/verdant-circuit/the-rise-of-solarpunk-art-where-optimistic-futures-bloom-on-canvas-564db17dfcb4).

---

## Couleurs (recherche + lecture sémantique)

Les références de palettes publiques convergent vers des **verts** (émeraude, lime, mousse), **jaunes / or / ambre** (soleil, récolte), **bleus ciel / eau / teal**, **crèmes / blancs cassés**, **terre** (brun, terre cuite), parfois **corail / orange chaud** en accent. Exemples de listes : [Colorany — palettes solarpunk](https://colorany.com/color-palettes/solarpunk-color-palettes/).

### Tableau sémantique (design system)

| Rôle | Interprétation solarpunk | Exemples de teintes (indicatif) |
|------|---------------------------|----------------------------------|
| Fond sombre | Forêt de nuit, canopée, profondeur | `#0a1410` → `#112920` |
| Vert primaire | Feuillage, croissance, « vie » | `#16a34a` – `#22c55e` – `#4ade80` |
| Or / ambre | Soleil, énergie, chaleur | `#f59e0b` – `#fbbf24` – `#fcd34d` |
| Eau / tech douce | Hydrique, futur calme | `#14b8a6` – `#2dd4bf` |
| Terre / chaleur | Argile, humanité | `#fb923c`, terre cuite |
| Erreur | Alerte uniquement (pas décoratif) | `#f87171` |
| Texte | Lisibilité sur fond sombre | `#ecfdf5` + muted vert menthe |

Les valeurs **canonical** pour le produit sont figées dans [solarpunk.tokens.css](../../../packages/theme/solarpunk.tokens.css) et `solarTheme`.

---

## Éléments visuels et narratifs caractéristiques

| Catégorie | Éléments typiques | Usage Video-AI / UI |
|-----------|-------------------|---------------------|
| **Formes** | Courbes, volutes, motifs végétaux, arches | `borderRadius` généreux en `solarTheme`, motifs SVG THP |
| **Nature** | Plantes, lianes, jardins verticaux | `ParticleField`, `Tree`, icônes Lucide (`Leaf`, `Sprout`…) |
| **Lumière** | Rayons, halos, golden hour | Dégradés radiaux, halos ambre |
| **Tech** | Panneaux solaires stylisés, réseaux « doux » | `FlowChart`, `Timeline`, code palette nature |
| **Mouvement** | Lent, organique | Springs `damping: 200`, particules lentes |
| **Philosophie** | Hopepunk, solutions | Ton THP : [thp-tone-and-theme](thp-tone-and-theme.md) |

---

## Typographie (recherche)

Références : [Solarpunk Typography Exposé (Clinamenic)](https://clinamenic.com/writing/A-Solarpunk-Typography-Expose), police display [Brassia](https://fontlibrary.org/en/font/brassia) (OFL). **Produit** : Poppins / Inter / JetBrains Mono dans `solarTheme` ; toute évolution documentée dans [solarpunk-theme-decisions](solarpunk-theme-decisions.md).

---

## Liste de choix — statut

Les dix questions initiales sont **tranchées** ; voir le tableau dans [solarpunk-theme-decisions](solarpunk-theme-decisions.md).

---

## Sources utilisées pour cette note

- [Wikipedia — Solarpunk](https://en.wikipedia.org/wiki/Solarpunk)
- [One Earth](https://www.oneearth.org/solarpunk/)
- [Aeon Essays](https://aeon.co/essays/in-solarpunk-cities-of-the-future-tech-follows-natures-lead)
- [Sound of Life — solarpunk aesthetic](https://www.soundoflife.com/blogs/design/solarpunk-aesthetic)
- [Number Analytics — solarpunk design guide](https://www.numberanalytics.com/blog/solarpunk-design-guide)
- [Medium — Verdant Circuit](https://medium.com/verdant-circuit/the-rise-of-solarpunk-art-where-optimistic-futures-bloom-on-canvas-564db17dfcb4)
- [Colorany — palettes](https://colorany.com/color-palettes/solarpunk-color-palettes/)
- [Clinamenic — Solarpunk typography](https://clinamenic.com/writing/A-Solarpunk-Typography-Expose)
- [Font Library — Brassia](https://fontlibrary.org/en/font/brassia)
