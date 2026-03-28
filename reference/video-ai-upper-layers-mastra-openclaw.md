---
title: "Video-AI — Couches supérieures : Mastra + OpenClaw (au-dessus de Trigger.dev v4)"
type: documentation
diataxis: explanation
status: published
area: video-ai
tags:
  - video-ai
  - mastra
  - openclaw
  - trigger-dev
  - architecture
  - hitl
created: 2026-03-27
updated: 2026-03-30
related:
  - "[[reference/video-ai-orchestrator-decision]]"
  - "[[02-video-ai-roadmap]]"
  - "[[research/workflow-tools-synthesis]]"
  - "[[research/mastra]]"
  - "[[research/openclaw]]"
  - "[[reference/video-lifecycle]]"
---

# Video-AI — Architecture des couches supérieures : Mastra + OpenClaw

**Prérequis** : **Trigger.dev v4** est l’**unique** orchestrateur du pipeline — voir [video-ai-orchestrator-decision](video-ai-orchestrator-decision.md).

**Objet de ce document** : cadrer **Mastra** (intelligence LLM **dans** des tasks Trigger) et **OpenClaw** (assistant **local**, hors pipeline prod), sans conflit architectural.

**Note méthodologique** : synthèse issue d’une **recherche externe** (ex. Perplexity). Recouper **APIs réelles** (Trigger `wait`, Mastra, OpenClaw) et **versions** au moment de l’implémentation ; les extraits de code sont **illustratifs**.

---

## Synthèse exécutive

Mastra et OpenClaw ont des rôles **orthogonaux** :

- **Mastra** : bibliothèque appelée **depuis** une task Trigger — **étape** du graphe uniquement, **jamais** orchestrateur parallèle ni alternative à Trigger — mémoire, tool-calling structuré, **HITL** possible via mécanismes Trigger (ex. tokens d’attente) — **sans** orchestrer les renders ni pousser seule en prod.
- **OpenClaw** : **poste auteur/dev**, périmètre d’accès **explicite**, **sans** chemin d’écriture vers le pipeline de **production**.

**Valeur Mastra** : surtout **après v1.1** (feedback API + données réelles à analyser). Avant cela, privilégier mocks minimaux ou appel LLM direct si besoin de prototyper.

---

<a id="mental-model-orchestrateur-agents"></a>

## Mental model : orchestrateur, agents, et périmètre Video-AI

*RFC produit — cadrage lecture ; complète la synthèse exécutive sans la remplacer.*

### Rôles : Trigger vs Mastra / agents

**Trigger.dev** = orchestrateur / scheduler (événements, retries, durée, observabilité) — esprit proche **Temporal-like**, en **TypeScript** natif.

**Mastra / agents** = couche de travail **cognitif** dans une **étape** (génération, structuration, tools), **invoquée depuis une task** Trigger — **pas** à la place de Trigger.

Sans couche « agent », Trigger sert déjà à des pipelines « bêtes » (webhooks, cron, ETL, render, notifications) : ce n’est **pas** vide, c’est une **autre** valeur métier que « dev agent qui commit ».

**Emboîtement** : 1 task = 1 workflow qui peut appeler **1+** agents, même si on code par couches (stub → lecture seule → diff → écriture derrière review).

### Video-AI n’est pas « dev agent repo-first »

Un agent dev qui touche à `src/`, Git, PR, etc. est orienté **auto-dev sur monorepo**. Ce n’est **pas** la priorité produit actuelle de Video-AI.

Le **cœur** Video-AI, aujourd’hui : rendu Remotion, collecte de **feedback**, re-render, **human-in-the-loop** — avec Mastra pour **analyser** le feedback et proposer des changements **structurés**, pas forcément un « Cursor autonome sur tout le monorepo » en v1.

### Analogie « orchestrateur sans runners »

- Produit **auto-dev sur repo** → sans agent cognitif, Trigger est surtout une **coquille** côté valeur « dev qui écrit le code ».
- **Video-AI pré-v2** → les « runners » actuels sont surtout **prepare / render (stub) / notify** ; la valeur immédiate = **fiabiliser** render et infra Trigger, pas encore l’**agent scripteur**.

### Mapping roadmap (sans incohérence)

1. **Trigger seul (pré-v2 actuel)** : valider fondamentaux (événements, observabilité, sécu/self-host, contrats `POST /jobs/…`) avec tasks **stub / non-LLM** — volontaire.
2. **Données de feedback (v1.1)** : la valeur cognitive devient pertinente ; ajouter LLM direct dans une task ou Mastra, d’abord **lecture / analyse** (peu ou pas d’écriture repo).
3. **Modification de contenu** : diff / PR / branche, ou patch validé — là **Trigger + agent** forment un **binôme** pour toucher au repo ou aux paramètres de render.

### Reco pragmatique (Video-AI)

- Concevoir dès maintenant le **binôme** Trigger ↔ couche IA (même si l’IA arrive plus tard) : une task = frontière claire **input / output / retries** ; l’agent vit **à l’intérieur** de cette frontière.
- **Faible épaisseur** au début : stub = OK pour valider infra Trigger ; prochain palier valeur = **feedback**, puis task d’analyse **read-only**, puis Mastra si la complexité le justifie.
- Un **agent dev sur repo** (style Cursor / OpenClaw comme couloir) = **distinct** ou étape ultérieure — ne pas mélanger « pipeline vidéo » et « agent qui réécrit tout le front » sans garde-fous.

<a id="progressive-agent-capabilities"></a>

### Escalade des capacités agent (2026)

**Doctrine d’implémentation** — à distinguer de la [roadmap](../02-video-ai-roadmap.md) (vue **d’avancement** : cette page porte la **règle d’escalade**, pas les cases à cocher).

- **Validé en principe** : **Trigger.dev** comme ossature — workflows durables, retries, observabilité, jobs longue durée.
- **Non validé en prod** : montée **d’un seul bloc** — Trigger + Mastra + **writer** sur le dépôt.
- **Mastra tôt, sans ambiguïté** : Mastra peut être présent **tôt en design ou en dépendance**, **mais sans droits d’écriture repo** tant que le périmètre n’est pas prêt — évite la lecture « on branche déjà un writer » alors que la cible d’abord est read-only ou diff.

**Échelle d’activation** :

1. **Trigger seul (pas d’agent autonome)** — orchestration utile **sans** couche agentique : **stub ou rendu minimal réel** (les deux sont possibles à ce palier). Ne pas confondre « pas d’IA » avec « orchestration inutile ».
2. **Mastra read-only** — analyse, scoring, propositions typées ; **pas** d’écriture repo.
3. **Write-to-diff** — patchs, PR, branche dédiée.
4. **Writer limité** — garde-fous, tests, review (humaine ou pipeline).
5. **OpenClaw** — optionnel, atelier local, pas cœur prod — [§ C OpenClaw](#c-openclaw--poste-auteur).

**Phrase clé** : la décision structurante est **quand l’agent obtient le droit d’agir sur le dépôt**, pas le choix « Trigger ou Mastra ».

**Cohérence pré-v2** : le **palier 1** correspond au **pré-v2** actuel ; les **paliers 2+** s’alignent sur **v1.1** puis **v2**. Tout **spike** read-only hors feuille de route doit être **borné** explicitement en doc.

### En résumé

**Trigger sans IA** verrouille déjà les **workflows durables** (retries, observabilité, enchaînement) — ce n’est pas « seulement » de la coquille infra. **Trigger + agents** = système **emboîté** quand on ajoute la couche cognitive dans une **task**. Video-AI = **pipeline vidéo orchestré** ; l’**IA enrichit les tasks**, elle ne **remplace** pas Trigger.

**Règle équipe (Mastra)** : **étape** du graphe Trigger (`task` → agent), **jamais** orchestrateur parallèle.

---

## Tableau des couches

| Couche | Outil | Responsabilité | Ce qu’il ne fait pas |
|--------|--------|----------------|---------------------|
| Orchestration pipeline | **Trigger.dev v4** | Déclenche, séquence, retry, observe chaque étape | Logique LLM, décision de contenu pédagogique |
| Intelligence LLM | **Mastra** (dans une task Trigger) | Analyse feedback, suggestions script/scène, scoring, mémoire par vidéo | Orchestrer les renders, second bus d’événements |
| Stockage métier | **PostgreSQL + Drizzle** | Source de vérité : jobs, statuts, feedback, outputs | — |
| Assistant auteur | **OpenClaw** (local) | Outline, exploration repo, CLI dev | Déclencher un render **prod**, accès secrets **prod** |

---

## A. Rôles et frontières

### Confier à Mastra

- Analyse de feedback en langage naturel → score structuré + suggestions typées (ex. Zod).
- Génération / correction de script ou scène (prompt → sortie structurée).
- Priorisation / scoring d’une file de vidéos (plusieurs analyses en parallèle côté Trigger).
- Mémoire inter-sessions par `videoId` (historique feedbacks).
- Enchaînement multi-agents (ex. « analyse » puis « rédacteur ») **toujours invoqué depuis une task Trigger**.

### Garder en code classique (task Trigger)

- Paramètres Remotion (bundle, `compositionId`, chemin sortie).
- `renderMedia()` ou `remotion render`.
- Upload artefact vers stockage.
- Mise à jour statut en base.
- Webhook / notification.
- Retries sur erreurs FFmpeg, OOM Chrome, etc.

### Anti-pattern : Mastra comme second orchestrateur

**Interdit** : enchaîner avec des **Mastra Workflows** qui appellent `triggerAndWait` / `task.trigger` — deux graphes parallèles difficiles à déboguer.

**Règle** : Mastra = **agents + tools** uniquement. Pas de workflow Mastra qui **déclenche** Trigger.

**Sens unique** : `Trigger task` → `mastra.getAgent(...).generate(...)` — **pas** l’inverse.

La doc Trigger sur les **AI agents** décrit des **étapes agentiques exécutées dans** un workflow durable Trigger — le socle reste **Trigger** (planification, durée, retries) ; Mastra ne joue **pas** le rôle d’orchestrateur parallèle.

### Garde-fous HITL et idempotence

- Utiliser les primitives **Trigger.dev** pour pause humaine (ex. **tokens d’attente** / équivalent selon doc officielle — valider le nom exact `wait.*` à l’implémentation).
- **Zod** (ou `outputSchema`) sur toutes les sorties agent.
- **Seuil de confiance** : pas de re-render auto si le score est sous le seuil configurable.
- **Idempotence** : `videoId` + hash du feedback dans les payloads d’approbation pour éviter doubles clics.
- **TTL** sur l’attente humaine (ex. 24 h) → annulation propre si pas de réponse.

**Exemple illustratif** (API exacte à vérifier dans la doc Trigger v4) :

```typescript
// Concept : pause jusqu'à approbation humaine, puis re-render
const token = await wait.createToken({ timeout: "24h" });
await notifyAuthor({ suggestion: mastraOutput, tokenId: token.id });
const approval = await wait.forToken<{ status: "approved" | "rejected" }>(token.id);
if (approval.ok && approval.output.status === "approved") {
  await renderVideoTask.triggerAndWait({ videoId, params: mastraOutput.params });
}
```

---

## B. Intégration Trigger.dev v4 + Mastra

### Patterns d’appel

**Pattern 1 — Une task avec Mastra inline** (POC) :

```typescript
import { task } from "@trigger.dev/sdk";
import { mastra } from "../mastra/index";

export const analyzeFeedbackTask = task({
  id: "analyze-feedback",
  retry: { maxAttempts: 3, factor: 2, minTimeoutInMs: 5000 },
  run: async (payload: { videoId: string; feedbackText: string }) => {
    const agent = mastra.getAgent("feedbackAnalyzer");
    const result = await agent.generate(payload.feedbackText, {
      threadId: payload.videoId,
      resourceId: payload.videoId,
    });
    return { suggestion: result.text, structured: result.object };
  },
});
```

**Pattern 2 — Sous-tâches Trigger** (v2, multi-étapes) :

```typescript
export const feedbackPipelineTask = task({
  id: "feedback-pipeline",
  run: async ({ videoId, feedback }) => {
    const analysis = await analyzeFeedbackTask.triggerAndWait({
      videoId,
      feedbackText: feedback,
    });
    const score = await scoreVideoTask.triggerAndWait({
      videoId,
      analysis: analysis.output,
    });
    if (score.output.confidence < 0.8) {
      const token = await wait.createToken({ timeout: "24h" });
      await notifyAuthor({ score, tokenId: token.id });
      await wait.forToken(token.id);
    }
    await renderVideoTask.trigger({ videoId, params: analysis.output.params });
  },
});
```

### Timeouts, rate limits

- `maxDuration` sur la task d’analyse LLM (ex. ~120 s — ajuster selon modèle).
- `concurrencyLimit` sur les tasks qui appellent l’API LLM pour éviter rafales 429.
- Erreurs 429 : laisser **Trigger** gérer les retries sur tasks idempotentes.

### Placement monorepo

Colocaliser **Mastra** avec le **code des tasks** sous **`apps/trigger`** (ex. `apps/trigger/src/trigger/mastra/`) pour que le **worker Trigger** exécute l’agent sans hop HTTP vers l’API Hono. L’**API** (`apps/api`) reste un client `tasks.trigger` vers la plateforme.

**Attention** : la doc Mastra + workspaces Bun évolue — valider la structure au moment du spike.

```text
apps/trigger/src/trigger/
  renderPipeline.ts  # tasks existantes
  mastra/            # v2 : agents / tools côté worker
    agents/
    tools/
packages/contracts/  # types partagés (pas de secrets)
apps/api/src/          # Hono uniquement ; pas de src/trigger
```

### Secrets LLM

- Clés **uniquement** côté **worker Trigger** (et jamais dans le frontend ni dans des packages commités).
- Injecter via **Coolify Secrets** / variables chiffrées — pas de secrets dans `docker-compose.yml` versionné.
- Rotation, principe de moindre privilège, pas de clé « admin » LLM.

---

## C. OpenClaw — poste auteur

**Périmètre** : **poste auteur local** avec surface d’action **limitée** et **explicite**. OpenClaw **n’a pas** d’accès direct à l’environnement **prod** ni à la **configuration des workflows Trigger** (`trigger.config`, secrets orchestration prod) — ce n’est **pas** un socle central d’orchestration à côté de Trigger. Les guides sécurité pour agents à fort périmètre (shell, réseau, skills) imposent ce **cloisonnement** ; sinon la surface de risque explose.

### Cas d’usage raisonnables

| Cas | Description |
|-----|-------------|
| Rédaction outline | Draft script depuis un brief → fichier local `scripts/draft.md` |
| Exploration repo | Lister composants Remotion, chemins |
| CLI dev | `bun run dev` **local** |
| Diff / PR | Résumer des changements locaux |
| Fixtures | Fichiers de test sous `apps/remotion/fixtures/` |

### Lignes rouges

- **Garde-fou** : **pas** d’accès direct à la **prod** ni à la **configuration des workflows Trigger** (`trigger.config`, secrets d’orchestration prod) — répété ici pour lecture rapide (voir § **Périmètre** ci-dessus).
- Ne **pas** déclencher render **prod** ni appeler API prod avec token.
- Ne **pas** exposer `DATABASE_URL` prod, `TRIGGER_SECRET_KEY`, clés LLM prod.
- Pas de **commit/push** vers `main` sans revue humaine.
- Pas de **migrations** prod depuis OpenClaw.

### Runbook permissions (à créer : `runbook/openclaw-permissions.md`)

- `DATABASE_URL` → local ou staging uniquement.
- `API_BASE_URL` → localhost en dev, jamais URL prod pour actions d’écriture.
- Outils : `read_file` large ; `write_file` limité aux zones repo autorisées ; **pas** `.env.production`.
- **Revue obligatoire** avant commit généré, changement **`apps/trigger/trigger.config.ts`**, nouveaux types dans `contracts`.
- Risque **prompt injection** via contenu fichier : ne pas ingérer du feedback utilisateur brut sans garde-fous.

#### Sécurité / permissions — rappel (poste auteur)

- **Pas** d’accès direct à la **prod** depuis le poste auteur (OpenClaw).
- **Pas** de modification des **workflows Trigger** — configuration, déploiement d’orchestration — depuis le poste auteur.

---

## D. Flux v2 cible (schéma)

```text
Feedback API (POST …/feedback)
        │ task.trigger()
        ▼
┌───────────────────────────────────────┐
│ Trigger.dev — feedback-pipeline       │
│   Step 1: analyze (Mastra agent)      │
│   Step 2: gate (auto ou HITL token)   │
│   Step 3: renderVideo (code classique)│
│   Step 4: notify + update DB          │
└───────────────────────────────────────┘
```

- **Trigger** = durabilité, retries, observabilité.
- **Mastra** = contenu des étapes LLM uniquement.
- **HITL** = primitive Trigger (token / attente), pas un graphe Mastra parallèle.

---

## E. Ordre d’introduction

### Mastra avant ou après la feedback API ?

**Recommandation** : intérêt **faible avant v1.1** (pas de données feedback réelles). Séquence possible :

1. **v1.0** : render pipeline sans IA (Trigger stub → render → notify).
2. **v1.1** : API feedback + stockage ; task d’analyse avec **LLM direct** (Vercel AI SDK, etc.) si besoin rapide.
3. **v1.2+** : migrer vers **Mastra** quand mémoire multi-run ou multi-agents est justifiée.
4. **v2.0** : boucle complète feedback → Mastra → HITL → re-render.

### POC Mastra (5 étapes suggérées)

1. Task mock `analyze_feedback` + déclenchement depuis Hono.
2. Remplacer par **LLM direct** + validation Zod.
3. Introduire **Mastra** + agent + mémoire (PostgreSQL selon doc Mastra).
4. Ajouter **HITL** (token / approbation) + endpoint d’approbation.
5. Observabilité (logs, TRQL / dashboards Trigger, replay).

### Adoption OpenClaw

1. Sandbox **local** uniquement, env limité.
2. Rédaction outline / exploration — itérer sur prompts système.
3. Runbook équipe + label Git `ai-assisted` sur commits aidés + session d’onboarding.

---

## F. Risques (Mastra + LLM)

| Risque | Mitigation |
|--------|------------|
| Coût tokens | Budgets, limites, alertes fournisseur |
| Non-déterminisme | `temperature` bas pour scoring ; plus haut seulement pour tâches créatives |
| Fuite PII dans prompts | Anonymiser / tronquer feedback avant injection |
| Panne fournisseur LLM | Fallback modèle, circuit-breaker |
| Prompt injection (feedback) | Sanitisation, instructions système, pas d’exécution d’instructions utilisateur |
| Dérive « Mastra orchestre » | Revue PR : interdire workflows Mastra qui déclenchent Trigger |

---

## G. Mastra vs appel LLM direct

| Critère | `generateText()` direct | Mastra `Agent.generate()` |
|---------|-------------------------|---------------------------|
| Mémoire par vidéo | À construire à la main | Souvent intégré (`threadId`, storage) |
| Multi-agents | Code lourd | Patterns agents + tools |
| Schéma sortie | Zod manuel | `outputSchema` côté agent |
| Complexité ops | Minimale | + dépendances, stockage mémoire |

**Règle** : direct tant qu’un seul appel sans mémoire inter-run suffit ; **Mastra** quand l’historique ou la coordination multi-agents apporte une valeur mesurable post-v1.1.

---

## H. Décisions à trancher en atelier

- Seuil **confidence** pour auto vs HITL (ex. 0,85 — à calibrer métier).
- **Modèle LLM** (coût, latence, souveraineté).
- **Schéma mémoire Mastra** : même Postgres avec schéma dédié vs instance séparée — gouvernance DB.
- **Granularité HITL** : toute suggestion vs seulement renders coûteux / longs.
- **Périmètre OpenClaw** : postes individuels vs environnement partagé ; gestion des clés et des logs.
- **Rotation des clés** LLM et **propriétaire** (compte service vs perso).
- **Gate** : valider livraison **v1.1** avant d’investir fortement dans Mastra.

---

## Voir aussi

- [video-ai-orchestrator-decision](video-ai-orchestrator-decision.md)
- [02-video-ai-roadmap](../02-video-ai-roadmap.md) — § v1.1, v2
- [workflow-tools-synthesis](../research/workflow-tools-synthesis.md)
- [mastra](../research/mastra.md) · [openclaw](../research/openclaw.md)
