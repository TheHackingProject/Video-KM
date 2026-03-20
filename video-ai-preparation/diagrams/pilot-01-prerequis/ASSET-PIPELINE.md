---
title: "Pilot 01 — pipeline assets diagramme (checklist)"
type: documentation
status: draft
area: video-ai
updated: 2026-03-23
---

# Pilot 01 — Pré-requis : pipeline diagrammes / SVG

Checklist **versionnée** pour tout **nouveau** diagramme Mermaid ou SVG utilisé par la composition `Pilot01Prerequis`.  
Ordre **obligatoire** : source `.mmd` → génération fichier dans `public/diagrams/` → Storybook (si nouveau bloc statique) → démo Remotion (si nouveau pattern) → doc / matrix.

**Références** : [video-ai-development §03b — item 3bis](../../../runbooks/video-ai-development.md) ; skill [thp-video-generation](../../../../../packages/skills/thp-video-generation/SKILL.md) ; [diagram-asset-pipeline](../../../../../packages/skills/thp-video-generation/references/diagram-asset-pipeline.md) (EN).

## Livrable V0.6 (timeline / texte)

Pour **V0.6**, aucun **nouveau** flux Mermaid → SVG n’est imposé dans ce plan (correction `Sequence`, layout, taxonomie titre). Cocher explicitement la ligne N/A ci-dessous évite l’ambiguïté en revue.

| Étape | Statut | Notes / lien |
| ----- | ------ | ------------ |
| Nouveau `.mmd` versionné sous ce dossier | **N/A V0.6** | Pas de nouveau diagramme Mermaid prévu pour V0.6. |
| SVG généré → `apps/remotion/public/diagrams/pilot-01-prerequis/` | **N/A V0.6** | À remplir si un asset est ajouté plus tard. |
| Storybook (si nouveau bloc statique) | **N/A V0.6** | |
| Démo Remotion + `Root.tsx` (si nouveau pattern motion) | **N/A V0.6** | |
| Runbook démos / `library-matrix.md` à jour | **N/A V0.6** | Obligatoire si une ligne ci-dessus n’est plus N/A. |

## Revue « ordre respecté » (agent ou humain)

- [ ] Aucune démo Remotion ni nouvelle story **avant** fichier source + sortie `public/` (sauf exception documentée ici : _____ ).
- [ ] Si une étape a été sautée : cause documentée (skill non chargé, demande utilisateur de raccourci, etc.).

## Futurs diagrammes (réutiliser ce tableau)

Pour chaque nouvel asset, ajouter une **ligne** ou une sous-section avec : nom du fichier `.mmd`, commande utilisée (`bunx @mermaid-js/mermaid-cli` ou skill pretty-mermaid), chemin SVG final, Storybook / démo (lien ou N/A), doc mise à jour (oui/non).
