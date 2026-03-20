---
title: "Brief — jeu d’icônes SVG THP / Solarpunk (pour designer ou agent pro)"
type: documentation
diataxis: reference
status: draft
area: design
tags:
  - thp
  - svg
  - icons
  - solarpunk
  - brief
created: 2026-03-19
updated: 2026-03-19
related:
  - "[[reference/solarpunk-theme-decisions]]"
  - "[[reference/solarpunk-theme-reflection]]"
---

# Brief : icônes SVG THP (Video-AI / Solarpunk)

## 0. Recommandation par défaut : bibliothèque (Lucide)

Pour les **pictogrammes UI** (titres, badges, scènes Remotion), le repo utilise en priorité **`lucide-react`** : composants **React** prêts à l’emploi, **grille 24 px**, **stroke 2** standard, pas de maintenance de paths SVG maison.

- **Import côté app** : `import { ThpSun, ThpTerminal, THP_LUCIDE_MAP } from "@repo/ui/icons"` — fichier [`packages/ui/src/thp-lucide.ts`](../../../packages/ui/src/thp-lucide.ts).
- **Doc / catalogue** : [lucide.dev/icons](https://lucide.dev/icons/).

Ce **brief** sert surtout si vous **commandez** un travail **sur mesure** (marque forte, picto signature hors Lucide, illustration plus riche). Les fichiers livrés pourront aller dans `packages/ui/src/assets/thp/svg/` (dossier réservé, vide tant qu’il n’y a pas de signature vectorielle).

---

Ce document sert à **commander un travail pro** (designer graphique, illustrateur·ice UI, ou agent spécialisé icônes) lorsque Lucide ne suffit pas. Il explique **ce qu’est le besoin**, **ce que la première version interne a tenté de faire**, **pourquoi elle ne suffit pas**, et **ce qu’on attend** à la place.

---

## 1. Contexte produit

- **Projet** : Video-AI — vidéos pédagogiques pour **The Hacking Project (THP)** (développement web, Git, terminal, etc.).
- **Rendu** : interfaces **dark** (fond type forêt profonde), composants **Remotion** (vidéo), Storybook, futur site THP.
- **Identité visuelle** : **Solarpunk** — futur **optimiste**, **écologie + technologie harmonieuses**, pas cyberpunk dystopique. Référence : [solarpunk-theme-reflection](../reference/solarpunk-theme-reflection.md), [solarpunk-theme-decisions](../reference/solarpunk-theme-decisions.md).

Les icônes doivent **renforcer cette identité** (organique, lumineux, soigné), pas ressembler à un pack générique « admin dashboard » ou à des pictogrammes trop géométriques froids.

---

## 2. Ce que sont les SVG dans ce projet

Ici, les SVG sont des **pictogrammes UI** (ligne ou plat léger), pas des illustrations pleine page. Ils servent à :

- remplacer ou compléter les **emoji** dans les titres, badges, en-têtes de scène ;
- **signifier** en un coup d’œil : terminal, Git, parcours d’apprentissage, énergie / nature ;
- rester **lisibles** à **16–24 px** dans les vidéos 1080p et sur mobile.

Format attendu : **SVG optimisés**, idéalement **monochrome** (`stroke="currentColor"` ou équivalent) pour teinter avec la palette verte / ambre / teal du thème.

---

## 3. Ce que la première version (interne) a voulu faire

**Objectif** : fournir un **lot 1** rapidement utilisable en code, avec :

- une **grille 24×24** ;
- des **traits arrondis** (`round` caps/joins) ;
- des **thèmes** : soleil, feuille, germe, fenêtre terminal, graphe Git, étincelle, panneau solaire, parcours d’apprentissage ;
- des fichiers `.svg` (+ intégration React au besoin après livraison).

**Intention** : couvrir les **métaphores pédagogiques** du repo (CLI, Git, progression) et **Solarpunk** (soleil, feuillage, panneau).

---

## 4. Limites de cette première version (pourquoi un pro est nécessaire)

La v1 a été produite **sans phase de design** (composition de formes simples, géométrie « schématique »). En pratique :

- **manque de personnalité** : rendu proche d’icônes système **génériques**, peu différenciant pour THP ;
- **faible alignement Solarpunk** : le mouvement Solarpunk évoque souvent **Art nouveau**, **courbes organiques**, **lumière**, **végétal stylisé** — pas seulement des traits droits et des cercles ;
- **cohérence optique** : les guides pros (voir §5) exigent **volume optique**, **densité** et **détails** alignés entre icônes — la v1 n’a pas été calibrée sur une **référence commune** (carré / cercle floutés, etc.) ;
- **stroke** : usage d’environ **1,75 px** au lieu du standard **2 px** courant sur grille 24 ([Lucide](https://lucide.dev/guide/design/icon-design-guide)) ;
- **détails** : certains éléments (ex. parcours avec pastilles pleines) peuvent **casser** la lisibilité à petite taille ou paraître **bruts** à côté d’icônes plus fines.

**Conclusion** : la v1 est un **placeholder technique** ; **elle ne remplace pas** un jeu d’icônes **designé** pour la marque.

---

## 5. Références professionnelles (à respecter ou s’en inspirer)

- **Lucide — Icon Design Guide** : [lucide.dev/guide/design/icon-design-guide](https://lucide.dev/guide/design/icon-design-guide) — grille 24 px, padding, **stroke 2 px**, **round** caps/joins, espacement entre éléments, volume optique, **pixel alignment**.
- **GitHub Primer — Octicons** : [primer.style/foundations/icons/design-guidelines](https://primer.style/foundations/icons/design-guidelines) — cohérence de set, usage sur fond sombre.
- **UX général** : équilibre reconnaissance / simplicité, test à **petite taille** ([uxplanet.org — Practical Guide To Icon Design](https://uxplanet.org/practical-guide-to-icon-design-794baf5624c4), etc.).

---

## 6. Direction artistique souhaitée (THP Solarpunk)

| Axes | Attendu |
|------|---------|
| **Ton** | Chaleureux, **espoir**, **clarté** pédagogique — jamais agressif ou « dystopique ». |
| **Formes** | Privilégier **courbes fluides**, **motifs végétaux** stylisés (sans surcharge), rappels **lumière** (rayons, halos) **sobres**. |
| **Tech** | La tech **s’intègre** à la nature (lignes douces, pas chrome froid seul). |
| **Cohérence** : toutes les icônes du **même famille** (même épaisseur, même « niveau de détail », même niveau d’abstraction). |

**Palette** (teinte via `currentColor`, pas obligatoirement dans le fichier) : verts `#22c55e` / `#6ee7b7`, ambre `#f59e0b` / `#fbbf24`, teal `#14b8a6` — fonds types `#0a1410`, `#071311`.

---

## 7. Liste des concepts à couvrir (lot 1 ou équivalent)

À **redessiner** ou **remplacer** par des propositions plus fortes :

| Concept | Usage produit |
|---------|----------------|
| Soleil / énergie | Identité « solar », optimisme |
| Feuille / nature | Croissance, environnement |
| Germe / pousse | Début de parcours, onboarding |
| Terminal / CLI | Pré-requis commandes, démos Git |
| Git branch | Série Git, branches |
| Étincelle / moment clé | Accroches, highlights |
| Panneau solaire / énergie renouvelable | Solarpunk « tech verte » |
| Parcours / étapes | Progression leçon, module |

Le designer peut **proposer des variantes** (ex. `terminal` en mode « fenêtre + invite » vs « curseur seul ») si c’est plus lisible ou plus marqué.

---

## 8. Contraintes techniques (livraison)

- **Fichiers** : SVG **propres**, optimisés (pas de `transform` inutiles, pas de `id` globaux dupliqués si embed en masse).
- **Attributs** : `viewBox="0 0 24 24"`, `fill="none"` si ligne ; **`stroke="currentColor"`** (ou équivalent) pour recoloration en UI.
- **Nommage** : `thp-{concept}.svg` en **kebab-case** anglais (ex. `thp-leaf.svg`, `thp-git-branch.svg`).
- **Nom descriptif** : l’icône représente **l’objet** (ex. `sun`, `leaf`), pas seulement le cas d’usage marketing (cf. conventions Lucide).
- **Accessibilité** : prévoir **titre** ou **aria** côté intégration React (déjà le pattern dans le code interne) ; l’asset SVG peut rester neutre (`aria-hidden` si décoratif).

---

## 9. Livrables attendus du·de la professionnel·le

1. **Fichiers SVG** (lot 1) + éventuellement **Figma** (ou fichier source) avec **grille 24** et composants.
2. **Planche de comparaison** : toutes les icônes côte à côte sur **fond sombre** + test **16 px / 24 px / 32 px**.
3. **Note de design** (1 page) : choix de stroke, **rayon** de courbure, règles pour les **lots suivants** (badges, flèches, états erreur — **sans** utiliser la couleur erreur comme décoration).
4. Option : **variante** ou règle pour **états** (ex. `git-branch` « merged ») si pertinent.

---

## 10. Critères d’acceptation

- [ ] **Lisibilité** immédiate à 24 px sur fond `#0a1410`.
- [ ] **Cohérence** visuelle entre toutes les icônes du lot (même « poids »).
- [ ] **Personnalité THP / Solarpunk** reconnaissable, pas copie d’un pack open-source existant sans adaptation.
- [ ] **Compat** technique : `currentColor`, pas de dépendance à des polices externes dans le SVG.
- [ ] Alignement avec les **guides** Lucide / équivalent (stroke, grille, padding).

---

## 11. Références internes repo

- Dépôt cible pour SVG signature : `packages/ui/src/assets/thp/svg/` (voir README du dossier).
- Décisions thème : [solarpunk-theme-decisions](../reference/solarpunk-theme-decisions.md).

---

## Sources web (recherche pour ce brief)

- [Lucide — Icon Design Guide](https://lucide.dev/guide/design/icon-design-guide)
- [Primer — Octicons design guidelines](https://primer.style/foundations/icons/design-guidelines/)
- [UX Planet — Practical Guide To Icon Design](https://uxplanet.org/practical-guide-to-icon-design-794baf5624c4)

---

*Document rédigé pour expliquer l’écart entre une v1 technique et une identité visuelle aboutie ; à joindre à toute commande externe d’icônes THP.*
