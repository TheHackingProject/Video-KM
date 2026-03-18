---
title: "Mastra"
type: research
diataxis: explanation
status: draft
last_reviewed:
area: tooling
tags:
  - research
  - mastra
  - ai
  - agents
created: 2026-03-18
updated: 2026-03-18
related:
  - "[[01-index]]"
  - "[[research/README]]"
  - "[[research/trigger-dev]]"
  - "[[research/inngest]]"
  - "[[reference/video-lifecycle]]"
  - "[[explanation/video-ai-vision]]"
---

# Mastra

## Summary

**Mastra** est un framework TypeScript **tout-en-un pour construire des applications et agents IA**. Créé par l’équipe derrière Gatsby, il vise à faire passer du prototype à la production.

- **Site officiel** : https://mastra.ai  
- **Docs** : https://mastra.ai/docs  
- **GitHub** : https://github.com/mastra-ai/mastra (open-source, Apache 2.0)

### Ce que Mastra apporte

| Brique | Description |
|--------|-------------|
| **Agents** | Agents autonomes avec LLM + **tools** (appels API, BDD, fonctions métier) ; raisonnement, choix d’outils, itération jusqu’à une réponse. |
| **Workflows** | Moteur de workflows en graphe (`.then()`, `.branch()`, `.parallel()`), étapes séquentielles/parallèles/branchées ; **human-in-the-loop** (pause pour saisie utilisateur, reprise depuis état sauvegardé). |
| **Mémoire** | Working memory (contexte récent), recall sémantique, mémoire observationnelle ; stockage via adapters (ex. `@mastra/libsql`). |
| **RAG / MCP** | RAG (retrieval-augmented generation), support Model Context Protocol (MCP), évaluation, observabilité (traces, usage tokens). |
| **Modèles** | Interface unifiée vers 40+ providers (OpenAI, Anthropic, Gemini, etc.). |
| **Intégration** | Next.js, Express, Hono, SvelteKit, Astro, React ; ou service standalone. Démarrage : `bunx create-mastra`. |

**À ne pas confondre** : Mastra n’est **pas** un orchestrateur de jobs longue durée type Inngest ou Trigger.dev. Il sert à **construire la couche “intelligence”** (agents, outils, raisonnement) ; l’exécution durable (rendu vidéo, retries, cron) reste du ressort d’un moteur de workflows (Inngest, Trigger.dev) avec lequel Mastra peut s’intégrer.

## Evaluation

### Contexte Video-AI (rappel)

- **Objectif** : produire et faire évoluer des vidéos pédagogiques THP (Remotion → rendu → intégration plateforme).
- **Cycle** : idée → préparation (formats, script) → composants (remotion-lib) → composition → review → render → THP → feedback → itération.
- **Vision** : v1 = base de vidéos manuelles ; v2 = feedback + IA pour améliorer ; v3 = régénération guidée / semi-autonome.
- **État actuel** : pas d’IA dans le pipeline (pas d’agent, pas de workflow asynchrone) ; rendu local/CLI ; boucle feedback “à venir” (runbook §05).

### Où Mastra est pertinent

Mastra est pertinent pour la **couche agent IA** dans les phases **v2 et v3** du lifecycle :

1. **Priorisation à partir du feedback**  
   Agent qui lit les retours apprenants/staff, les agrège et propose quelles vidéos retravailler en priorité (tools : accès BDD feedback, formats pédagogiques).

2. **Proposition de changements de script / scène**  
   Agent qui suggère des modifications de script ou de structure de scène à partir du feedback ou des objectifs pédagogiques (mémoire du contexte cours, formats dans `video-ai-preparation`).

3. **Brouillons et assistance à l’édition**  
   Génération de brouillons de texte pour cartes, voix off, ou suggestions de composants Remotion à utiliser (tools : lecture pilot-outline, shortlist composants).

4. **Veille / analyse de commentaires**  
   Schémas type “agent IA analyser les commentaires” (Excalidraw Monorepo) : un agent Mastra avec outils (API commentaires, cours) et mémoire peut tenir ce rôle.

Ces cas supposent : **un agent** (LLM + tools + mémoire), pas seulement un job batch. D’où l’intérêt d’un framework agents comme Mastra plutôt que d’appels LLM ad hoc.

### Où Mastra ne suffit pas (et ne remplace pas)

- **Orchestration de jobs** : lancement de rendu Remotion, retries, file d’attente, cron → Inngest ou Trigger.dev (voir `research/trigger-dev.md`, `research/inngest.md`).
- **Exécution longue et fiable** : un workflow “feedback reçu → priorisation IA → décision re-render → lancer le rendu” doit s’appuyer sur un moteur durable (étapes persistées, reprise sur échec). Mastra peut être **une étape** (l’agent de priorisation) dans un workflow Inngest ou Trigger.dev.

En résumé : **Mastra = couche “intelligence” (agents, tools, mémoire) ; Inngest/Trigger.dev = couche “exécution fiable” (workflows durables, rendu, planification).**

### Fit avec la stack actuelle

- **Langage** : TypeScript (Bun, Remotion, Next/Storybook) → excellent alignement.
- **Pas de backend dédié aujourd’hui** : les agents Mastra seraient invoqués soit depuis une future app (ex. `apps/web`), soit comme **steps** dans des workflows Inngest/Trigger.dev.
- **Intégrations existantes** : Trigger.dev propose un guide “[Mastra agents with memory + Trigger.dev task orchestration](https://trigger.dev/docs/guides/example-projects/mastra-agents-with-memory)” ; Mastra propose un adapter pour **Inngest** (workflows durables). Donc l’intégration de Mastra dans un process déjà orchestré par Inngest ou Trigger.dev est prévue et documentée.

### Verdict par version

| Version | Rôle de Mastra | Recommandation |
|---------|----------------|-----------------|
| **v1** | Aucun (pas d’agent IA dans le process) | **Ne pas introduire Mastra** ; priorité = compositions, design system, runbook de rendu simple. |
| **v2** | Agents de priorisation, suggestions de script/scène, assistance à l’édition à partir du feedback | **Candidat pertinent** pour la couche agent, en complément d’un orchestrateur (Inngest ou Trigger.dev) pour la partie rendu et planification. |
| **v3** | Agents plus autonomes (régénération guidée, propositions de mises à jour) | **Très aligné** : mémoire, tools, workflows (dont human-in-the-loop) correspondent au besoin “régénération dans des garde-fous”. |

**Conclusion** :  
- **Utile / optimal** : **oui**, pour la partie **agents IA** du process (v2/v3), pas pour remplacer l’orchestration de jobs.  
- **Optimalité** : Mastra est un bon choix pour centraliser agents + tools + mémoire en TypeScript, tout en s’appuyant sur Inngest ou Trigger.dev pour la durabilité et l’observabilité des pipelines (rendu, feedback → re-render).

## Comparison / alternatives

### Mastra vs Inngest / Trigger.dev

- **Mastra** : framework pour **agents** (LLM, tools, mémoire, raisonnement). Il peut utiliser **Inngest** ou **Trigger.dev** pour exécuter des workflows durables (ex. “run this agent as a step”).
- **Inngest / Trigger.dev** : orchestration de **jobs/workflows** (retries, cron, événements, long-running). Ils n’implémentent pas la logique “agent avec tools et mémoire”.

Donc **complémentaires** : on peut avoir “Mastra (agent) + Inngest” ou “Mastra (agent) + Trigger.dev” selon le choix d’orchestration (voir recherches trigger-dev et inngest).

### Autres frameworks “agents”

- **LangChain / LangGraph** : écosystème riche, plutôt Python ; moins aligné avec une stack 100 % TypeScript comme Video-AI.
- **Vercel AI SDK** : excellent pour flux de streaming et UI, moins tourné “agent avec mémoire et workflows multi-étapes” que Mastra.
- **Custom (appels LLM + scripts)** : possible pour un premier POC très ciblé, mais on réinvente tools, mémoire et patterns workflow ; Mastra apporte une structure et des intégrations (dont Inngest/Trigger) utiles dès qu’on a plusieurs cas d’usage agents.

## Risks & constraints

- **Maturité** : projet actif (ex. 22k+ stars), mais l’écosystème évolue vite ; suivre les breaking changes et la compatibilité avec Inngest/Trigger.dev.
- **Surface à maintenir** : en plus du monorepo, Remotion et éventuellement Inngest/Trigger, une couche Mastra implique modèles, prompts, tools et éventuellement stockage mémoire (LibSQL ou autre).
- **Sécurité / données** : les agents peuvent accéder à des données pédagogiques et feedback ; à cadrer (politique THP, secrets, périmètre des tools).
- **Coût** : appels LLM selon le volume de feedback et de suggestions ; à dimensionner en v2/v3.

## POC / notes

Idées de POC ciblés **v2** (une fois qu’un premier pipeline feedback ou rendu existe) :

1. **Agent de priorisation**  
   - Input : extrait de feedback (ou mock) pour N vidéos.  
   - Agent Mastra avec un tool “liste des vidéos + métadonnées” et instructions “proposer un ordre de priorité avec justification courte”.  
   - Output : liste priorisée + raisons (pour affichage dans une future app ou en entrée d’un workflow).

2. **Agent “suggestions de script”**  
   - Input : identifiant vidéo + résumé du feedback.  
   - Agent avec accès (tool) au format et au pilot-outline type, + instructions pour proposer 2–3 modifications de script.  
   - Intégration possible comme **étape** d’un workflow Inngest/Trigger.dev “on feedback reçu → appeler l’agent → créer une issue ou une carte Kanban”.

3. **Mastra + Trigger.dev**  
   - Reproduire le pattern du guide officiel Trigger.dev “Mastra agents with memory” dans un mini-workflow : une tâche Trigger qui appelle un agent Mastra (avec mémoire partagée si besoin), puis enchaîne sur une action (ex. notification, création de tâche).  
   - Objectif : valider la DX et la robustesse de l’intégration dans le contexte Video-AI.

Critères de succès avant adoption formelle : complexité d’intégration raisonnable, maintenabilité par l’équipe, compatibilité avec le futur runbook de rendu et la boucle feedback (voir `runbooks/video-ai-development.md` §05).

## References

- Mastra – docs : https://mastra.ai/docs  
- Mastra – quickstart : `bunx create-mastra`  
- Mastra – workflows (graphe, human-in-the-loop) : https://mastra.ai/docs/workflows  
- Mastra – agents (overview, tools, memory) : https://mastra.ai/docs/agents/overview  
- Trigger.dev – Mastra + Trigger : https://trigger.dev/docs/guides/example-projects/mastra-agents-with-memory  
- Video-AI : [reference/video-lifecycle](../reference/video-lifecycle.md), [explanation/video-ai-vision](../explanation/video-ai-vision.md), [research/trigger-dev](trigger-dev.md), [research/inngest](inngest.md)  
- Décision d’adoption : documenter dans `reference/tools/mastra.md` et éventuellement un ADR une fois décidé.
