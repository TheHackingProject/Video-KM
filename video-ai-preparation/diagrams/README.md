# Diagrammes vidéo (Mermaid → SVG)

Convention : un dossier par **slug** vidéo sous `diagrams/<slug>/` avec :

- Fichiers source `.mmd` (diff/review).
- **`ASSET-PIPELINE.md`** — checklist d’ordre de travail (évite sauts d’étapes agent).
- **Diagrammes « type Mermaid » en React** : préférer **`SchematicFlowChartView`** + **`FlowChart`** (`@repo/ui/remotion`) — Storybook `THP/Diagrams/SchematicFlowChartView`, démo `DiagramsDemo` (pas d’image SVG minuscule au centre de l’écran).
- **Export Mermaid → SVG** : option conservée pour assets statiques lourds (voir [runbook §03b](../../runbooks/video-ai-development.md) + `ASSET-PIPELINE` par slug) ; ce n’est pas le pattern par défaut pour les leçons THP.
- Exemple checklist pilot : [pilot-01-prerequis/ASSET-PIPELINE.md](pilot-01-prerequis/ASSET-PIPELINE.md).

Les SVG rendus pour Remotion vont sous `apps/remotion/public/diagrams/<slug>/` ; détail : [runbook Video-AI Development §03b — 3bis](../../runbooks/video-ai-development.md).
