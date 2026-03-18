---
title: "Trigger.dev"
type: research
diataxis: explanation
status: draft
last_reviewed:
area: tooling
tags:
  - research
  - trigger-dev
created: 2026-03-18
updated: 2026-03-18
related:
  - "[[01-index]]"
  - "[[research/README]]"
---

# Trigger.dev

## Summary

Trigger.dev est une plateforme open‑source (v3) pour orchestrer des workflows et tâches de fond **longue durée** en TypeScript.  
On écrit des jobs comme de simples fonctions `async` (Node/Bun), avec persistance d’état, reprise après échec, planification, webhooks et intégrations (OpenAI, Slack, etc.).  
Site officiel : https://trigger.dev

## Evaluation

### Besoins potentiels dans Video-AI

À partir de `video-lifecycle` et du runbook `video-ai-development` :

- **Rendu vidéo** : exécuter des rendus Remotion (possiblement longs) de manière fiable, en batch, avec reprise après échec.  
- **Pipelines IA (v2/v3)** : génération de scripts, suggestions d’itérations, analyses de feedback apprenant, avec appels multiples à des APIs IA.  
- **Boucle feedback → re‑render** : déclencher des mises à jour de compositions / rendus à partir de signaux de feedback (tâches planifiées, events, re‑queues).  
- **Intégration avec THP / autres services** : webhooks, notifications, synchronisation avec la plateforme.

### Points forts de Trigger.dev pour ce contexte

- **Long‑running / IA‑friendly** : conçu pour des jobs qui peuvent durer plusieurs minutes/heures (problème classique avec les timeouts serverless) → adapté aux rendus Remotion lourds et pipelines IA.  
- **TypeScript‑first** : le repo Video-AI est déjà orienté TypeScript (Remotion, Next/Storybook), donc très bon fit pour l’équipe.  
- **Durable workflows** : chaque étape est persistée ; on peut reprendre un job au milieu d’un pipeline (utile pour orchestrer plusieurs appels IA + rendu).  
- **Open‑source & self‑host** : on garde la maîtrise des données et du coût infra (surtout si on veut exposer des infos internes THP / feedback).  
- **Observabilité intégrée** : suivi des runs, logs, étapes, ce qui aide à déboguer et monitorer les pipelines vidéo/IA.

### Points faibles / coûts

- **Complexité infra** : nécessite un déploiement dédié (workers, DB). Pour l’instant, le runbook mentionne un futur `video-ai-rendering.md` mais aucun pipeline automatisé n’est encore en place → risque de sur‑ingénierie en v1.  
- **Surface supplémentaire à maintenir** : configuration, upgrades, monitoring de Trigger.dev en plus du monorepo, de Remotion et du CI existant.  
- **Courbe d’apprentissage** : nouvelle API et modèle mental (workflows, runs, triggers) à maîtriser par l’équipe.

### Verdict préliminaire (par version)

- **v1 (actuel)** – Compositions Remotion locales, rendu manuel/CLI, peu ou pas d’automatisation :  
  - **→ Trigger.dev non nécessaire** à court terme.  
  - Priorité : stabiliser les compositions, le design system et un runbook de rendu simple (`video-ai-rendering.md` quand il sera écrit), éventuellement via scripts/CI simples.
- **v2 (feedback + IA pour améliorer les vidéos)** :  
  - **→ Trigger.dev devient un candidat sérieux** pour orchestrer :  
    - ingestion de feedback,  
    - appels IA (propositions de changements),  
    - planification de re‑rendus Remotion.  
- **v3 (régénération guidée / semi‑auto)** :  
  - **→ besoin fort** d’un orchestrateur de workflows IA + rendu + validation humaine. Trigger.dev est aligné avec ce besoin (durable, TS, long‑running).

Conclusion provisoire :  
- **Garder Trigger.dev en veille et préparer un petit POC ciblé pour v2**, mais **ne pas l’intégrer dans le cœur du process v1** tant que le pipeline de rendu et la boucle de feedback ne sont pas plus matures.

## Comparison / alternatives

### Inngest

Voir aussi `research/inngest.md` (squelette). Synthèse rapide par rapport à Trigger.dev :

- **Cloud‑first** : Inngest est principalement proposé en SaaS, très intégré aux plateformes serverless (Vercel, Netlify).  
- **Event‑driven step functions** : idéal si l’on a déjà une architecture très orientée événements (webhooks THP, events internes).  
- **Intégration rapide** : très peu de boilerplate pour démarrer, bonne option pour des workflows simples et à volume important.

Pour Video-AI :

- **Pour des besoins modestes de jobs asynchrones** (quelques rendus, quelques tâches périodiques) Inngest ou même un **CI + scripts Bun** peuvent suffire.  
- **Pour des workflows IA + rendu longs, à forte valeur** (v2/v3), le côté self‑host et long‑running de Trigger.dev est un meilleur fit que du pur SaaS.

### Temporal (référence du marché)

- Plateforme d’orchestration très mature, mais plus lourde à opérer (cluster, SDKs, etc.).  
- Sur‑dimensionnée pour l’instant pour Video-AI, tant que les pipelines restent relativement simples et peu nombreux.  
- Intéressant comme référence conceptuelle (workflows durables), mais **pas recommandé comme premier choix** pour ce projet.

## Risks & constraints

- **Opérationnel** : nécessite une stack d’hébergement (DB, workers) et un runbook d’exploitation (backups, monitoring, upgrades).  
- **Couplage au process** : si Trigger.dev devient le cœur de la boucle feedback → rendu, un changement d’outil plus tard aura un coût important (à documenter via ADR si adopté).  
- **Lock‑in modéré** : code en TS relativement portable, mais modèles de données (runs, steps, history) spécifiques.  
- **Sécurité / données** : attention aux données pédagogiques et au feedback apprenant transitant dans les workflows (à cadrer avec les politiques THP).

## POC / notes

Idées de POC ciblé (v2) :

- **POC 1 – Orchestration de rendu Remotion**  
  - Workflow Trigger.dev qui :  
    - reçoit un event « nouvelle composition à rendre »,  
    - lance un rendu Remotion (CLI ou API),  
    - stocke l’URL du résultat et notifie un canal (Slack / email).  
- **POC 2 – Pipeline IA simple sur feedback**  
  - Job qui lit un échantillon de feedback pour une vidéo, appelle un modèle IA pour proposer des pistes d’amélioration, et ouvre une tâche (GitHub issue / carte Kanban MCP) pour suivi.

Résultats attendus du POC avant toute adoption formelle :

- Mesurer la **complexité d’intégration** (temps de mise en place, friction dev).  
- Vérifier la **robustesse** sur des jobs de rendu longs.  
- Évaluer l’**ergonomie** de l’UI de monitoring pour l’équipe.

## References

- Trigger.dev – docs : https://trigger.dev/docs  
- Blog / comparatifs récents sur l’orchestration TypeScript (Trigger.dev v3, Inngest, Temporal, etc.)  
- Une fois une décision prise : créer une page dans `reference/tools/` et éventuellement un ADR dédié.
