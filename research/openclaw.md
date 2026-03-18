---
title: "OpenClaw"
type: research
diataxis: explanation
status: draft
last_reviewed:
area: tooling
tags:
  - research
  - openclaw
  - agent
  - automation
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

# OpenClaw

## Summary

**OpenClaw** est un agent IA **autonome**, open-source (MIT), qui tourne **en local** sur la machine de l’utilisateur. Il connecte des LLM (Claude, GPT, Grok ou modèles locaux) aux fichiers, au shell, au navigateur, aux messageries (WhatsApp, Slack, Telegram, etc.) et à des dizaines de services.

- **Site officiel** : https://openclaws.io
- **Documentation** : https://clawdocs.org (ex. [Introduction](https://clawdocs.org/getting-started/introduction/), [Heartbeat](https://clawdocs.org/architecture/heartbeat))

À la différence d’un simple chatbot, OpenClaw **agit** : il exécute des commandes, lit/écrit des fichiers, pilote le navigateur, appelle des APIs. Il dispose d’un **heartbeat** (tâches proactives toutes les N minutes), d’une **mémoire persistante** (Markdown local), et d’un écosystème de **skills** (ClawHub). Données et exécution restent sur la machine (self-hosted).

## Evaluation

### Besoins Video-AI (rappel)

D’après [video-lifecycle](reference/video-lifecycle.md) et [video-ai-vision](explanation/video-ai-vision.md) :

- **v1** : Création/maintenance manuelle des vidéos (compositions Remotion, rendu CLI, pas d’orchestration).
- **v2** : Boucle feedback + IA pour prioriser et proposer des améliorations ; re-rendus déclenchés à partir des signaux.
- **v3** : Régénération guidée / semi-autonome dans des garde-fous définis par l’équipe.

Le besoin principal pour **automatiser le process** (v2/v3) est un **orchestrateur de workflows** : jobs durables, étapes (préparation → rendu → post-traitement → publication), retries, événements (feedback, merge PR, cron). Voir [trigger-dev](trigger-dev.md) et [inngest](inngest.md).

### OpenClaw vs orchestration (Trigger.dev / Inngest)

| Critère | OpenClaw | Trigger.dev / Inngest |
|--------|----------|------------------------|
| **Rôle** | Agent IA qui *fait des choses* sur une machine (fichiers, shell, browser, APIs). | Moteur de **workflows** : jobs déclenchés par événements/cron, étapes durables, retries, observabilité. |
| **Modèle** | Proactif (heartbeat), conversationnel, “assistant qui exécute”. | Réactif : “quand X arrive, exécuter ce pipeline”. |
| **Où ça tourne** | Local (ou machine dédiée) avec accès complet. | Workers / serverless ; pas d’accès “bureau” par défaut. |
| **Cas d’usage typiques** | Triage email, scripts ad hoc, CRM, rapports, automatisation personnelle. | Rendu batch, pipelines IA multi-étapes, feedback → re-render, intégration THP. |

Pour Video-AI, **l’orchestration du pipeline** (rendu, feedback, re-rendu) relève naturellement d’un **moteur de workflows** (Trigger.dev ou Inngest), pas d’un agent conversationnel. OpenClaw ne remplace pas ces outils pour la partie “back-end” du process.

### Où OpenClaw pourrait être utile

- **Côté auteur / dev** (optionnel) :
  - Aide à la **rédaction de scripts** ou de pilot outlines (à partir du template et des formats).
  - Suggestions ou brouillons pour des compositions (avec garde-fou humain).
  - Exécution locale de commandes (ex. `remotion render`, scripts de build) depuis une interface conversationnelle.
- **Pas comme socle du pipeline** : le cœur du process (événements → jobs durables → rendu → THP) gagne à rester sur Trigger.dev ou Inngest, plus prévisibles et observables.

### Coût et complexité

- **OpenClaw** : gratuit (MIT) ; coût = LLM (≈ 1–5 $/j en usage léger, plus si heartbeat intense). Modèles locaux (Ollama, vLLM) = 0 $.
- **Complexité** : un agent avec accès fichiers/shell/browser augmente la surface d’attaque. La doc et la communauté mentionnent des incidents (CVE, skills malveillants). Il faut lire le [Security Guide](https://clawdocs.org/security/overview), partir en lecture seule et étendre les permissions progressivement.

### Verdict préliminaire (fit avec Video-AI)

- **Intégration d’OpenClaw dans le *cœur* du process (pipeline rendu / feedback / re-render)** : **non optimal**.  
  - Le besoin est **orchestration de workflows** (événements, étapes durables, retries), pas un agent conversationnel.  
  - **Recommandation** : utiliser Trigger.dev ou Inngest pour la partie pipeline (voir [trigger-dev](trigger-dev.md), [inngest](inngest.md)).

- **OpenClaw comme outil d’assistance aux auteurs (v1/v2)** : **optionnel, possible**.  
  - Utile si l’équipe veut un assistant local pour scripts, brouillons, ou lancement de commandes Remotion.  
  - À traiter comme un **outil à part** (poste dev / auteur), avec permissions limitées et revue humaine, pas comme la brique centrale du process.

En résumé : **OpenClaw n’est pas la bonne brique pour “intégrer l’automatisation dans le process” Video-AI** ; en revanche, il peut compléter le process en tant qu’**assistant local** pour la préparation et l’édition, si l’équipe le souhaite.

## Comparison / alternatives

- **Trigger.dev** : orchestrateur de jobs/workflows long-running, TypeScript, self-host possible. Adapté au pipeline rendu + IA (v2/v3). Voir [research/trigger-dev](trigger-dev.md).
- **Inngest** : workflows événementiels, multi-étapes, cloud-first. Alternative pour déclencher rendus et pipelines. Voir [research/inngest](inngest.md).
- **OpenClaw** : agent autonome local ; complémentaire plutôt qu’alternatif à Trigger/Inngest pour le cœur du process.

## Risks & constraints

- **Sécurité** : accès système large ; CVE et skills malveillants documentés. Nécessite configuration prudente et compréhension des permissions.
- **Maturité** : projet très actif mais histoire récente (rebrands, incidents). À suivre pour les mises à jour de sécurité.
- **Coût LLM** : heartbeat et usage intensif peuvent faire monter la facture API ; privilégier un modèle économique (ex. Haiku) ou modèles locaux pour les tâches répétitives.

## POC / notes

- Si POC “assistant auteur” : installer en local, limiter les skills et les permissions (fichiers en lecture seule au départ), tester rédaction de pilot outline ou appels CLI Remotion.
- Aucun POC OpenClaw n’est actuellement prévu dans le repo pour le pipeline principal.

## References

- [OpenClaw – Introduction](https://clawdocs.org/getting-started/introduction/)
- [OpenClaw – Heartbeat](https://clawdocs.org/architecture/heartbeat)
- [OpenClaw – Security](https://clawdocs.org/security/overview)
- [research/trigger-dev](trigger-dev.md) – Orchestration recommandée pour le pipeline Video-AI (v2/v3)
- [research/inngest](inngest.md) – Alternative event-driven pour workflows
