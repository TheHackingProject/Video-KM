---

## title: "Synthèse – Outils à intégrer au workflow Video-AI"
type: research
diataxis: explanation
status: draft
last_reviewed:
area: tooling
tags:
  - research
  - workflow
  - synthesis
  - inngest
  - trigger-dev
  - mastra
  - openclaw
created: 2026-03-18
updated: 2026-03-20
related:
  - "[[research/inngest]]"
  - "[[research/trigger-dev]]"
  - "[[research/mastra]]"
  - "[[research/openclaw]]"
  - "[[reference/video-lifecycle]]"
  - "[[explanation/video-ai-vision]]"

# Synthèse – Ce qui est vraiment utile à intégrer au workflow

Point sur les quatre recherches (Inngest, Trigger.dev, Mastra, OpenClaw) : **quoi intégrer, quand, et dans quel rôle**.

## En bref


| Outil           | Rôle                                                              | Intégrer au workflow ?                      | Quand                                                       |
| --------------- | ----------------------------------------------------------------- | ------------------------------------------- | ----------------------------------------------------------- |
| **Inngest**     | Orchestration event-driven (jobs, étapes durables, retries, cron) | **Oui, cœur du pipeline**                   | v2/v3 (pas v1)                                              |
| **Trigger.dev** | Idem (workflows long-running, TypeScript, self-host)              | **Oui, cœur du pipeline**                   | v2/v3 (pas v1)                                              |
| **Mastra**      | Couche **agents IA** (LLM, tools, mémoire, raisonnement)          | **Oui, couche “intelligence”**              | v2/v3, en complément de l’orchestrateur                     |
| **OpenClaw**    | Agent autonome **local** (fichiers, shell, browser)               | **Optionnel** (assistant auteur uniquement) | Si besoin d’un assistant local, jamais comme socle pipeline |


**Règle importante** : pour le **cœur du process** (rendu → feedback → re-render), il faut **un seul** orchestrateur : soit **Inngest**, soit **Trigger.dev**. Mastra s’ajoute **par-dessus** pour la partie IA. OpenClaw reste à part (outil dev/auteur).

**Hors cœur du pipeline ≠ « déprécié » ou inutile** : il s’agit d’un choix de **répartition des rôles** — orchestration durable et observable d’un côté, agent IA **dans le graphe** produit (Mastra) de l’autre, assistant **hors graphe** sur poste auteur/dev (OpenClaw en option). Ce n’est pas une hiérarchie « le plus créatif gagne », mais des **couches différentes**.

**Séparation des responsabilités (rappel)** :

- **Orchestrateur (Inngest ou Trigger.dev)** : événements, étapes durables, retries, observabilité.
- **Mastra** : agent LLM + tools **comme étape** du workflow (pas un second orchestrateur).
- **OpenClaw** : assistant local **en marge** du pipeline central ; utile pour explorer, brouillonner, lancer des CLI, sans servir de source de vérité du process batch.

---

## 1. Ce qui est vraiment utile et important

### À intégrer au workflow (v2/v3)

1. **Un orchestrateur de workflows (Inngest OU Trigger.dev)**
  - **Utile pour** : pipeline de rendu multi-étapes (prepare → render → post_process → publish → notify), boucle feedback → re-render, cron / planification.
  - **Important** : retries par étape, observabilité, jobs longue durée. Sans ça, on recode une queue + state machine maison (non aligné KISS).
  - **Quand** : à partir du moment où le rendu devient distant/batch et où la boucle feedback existe. **Pas en v1** (rendu local, volume faible = complexité en avance).
2. **Mastra (couche agents IA)**
  - **Utile pour** : priorisation à partir du feedback, suggestions de script/scène, brouillons, analyse de commentaires. Tout ce qui demande un **agent** (LLM + tools + mémoire), pas juste un job batch.
  - **Important** : Mastra ne remplace pas l’orchestrateur. Il est **une étape** dans un workflow Inngest/Trigger (ex. “feedback reçu → appeler l’agent Mastra → décision → lancer re-render”).
  - **Quand** : v2/v3, une fois qu’un premier pipeline (feedback ou rendu) existe.

### Optionnel / à part

1. **OpenClaw**
  - **Utile pour** : assistant **local** pour auteurs (rédaction de pilot outline, brouillons, lancement de `remotion render` en conversationnel). Données et exécution restent sur la machine. Peut être **très exploratoire / créatif** grâce au large périmètre d’action (fichiers, shell, navigateur).
  - **Pas pour** : le pipeline central (événements → jobs durables → rendu → THP). OpenClaw n’est pas un moteur de workflows ; l’orchestration reste Inngest ou Trigger.dev.
  - **OpenClaw vs Mastra (créativité)** : la **créativité cadrée dans le flux produit** (feedback → décision → re-render, traçabilité, reproductibilité) est confiée à **Mastra comme étape** derrière l’orchestrateur. OpenClaw peut coexister comme **outil de poste** sans remplir ce rôle dans le graphe.
  - **Si intégré** : comme outil à part (poste dev/auteur), permissions limitées, revue humaine, pas comme brique centrale.

---

## 2. Ce qu’il ne faut pas intégrer (ou pas maintenant)

- **Inngest ou Trigger.dev en v1** : sur-ingénierie tant que rendu local, pas de backend, volume modeste. Stabiliser d’abord lifecycle, format, runbook de rendu.
- **Mastra en v1** : pas d’agent IA dans le process actuel ; priorité = compositions, design system, runbook.
- **OpenClaw comme socle du pipeline** : il ne gère pas les workflows durables ni les événements ; ne pas le mettre au cœur du process.
- **Plusieurs orchestrateurs** : un seul (Inngest **ou** Trigger.dev) pour éviter double logique et double coût.

---

## 3. Ordre d’intégration recommandé


| Phase           | Action workflow                                                                                                                                   | Outils concernés                                                                                              |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **v1 (actuel)** | Rendu manuel/CLI, pas d’orchestration. Écrire/consolider le runbook de rendu (`video-ai-rendering.md`).                                           | Aucun de ces quatre dans le cœur du process. Optionnel : OpenClaw en assistant local si l’équipe le souhaite. |
| **Pré-v2**      | Définir le runbook de rendu distant (Remotion Lambda ou équivalent). POC orchestration : un workflow minimal (event → prepare → render → notify). | **POC** Inngest **ou** Trigger.dev ; choisir un des deux après comparaison (DX, prix, self-host).             |
| **v2**          | Pipeline feedback + IA : ingestion feedback → priorisation/suggestions IA → re-rendus.                                                            | **Orchestrateur** (Inngest ou Trigger.dev) + **Mastra** comme étape “agent” (priorisation, suggestions).      |
| **v3**          | Régénération guidée / semi-autonome, human-in-the-loop.                                                                                           | Même stack ; Mastra workflows (human-in-the-loop) + orchestrateur pour rendu et planification.                |

**v1 et prod disciplinée** : la fabrication actuelle des vidéos (runbooks, matrice biblio → Storybook → démo Remotion, garde-fous design) reste la référence **humaine et reproductible**. L’orchestration v2/v3 **enveloppe** ce travail (rendu distant, boucles feedback) ; elle ne remplace pas la structure runbook / skill côté monorepo.

---

## 4. Décisions à trancher plus tard

- **Inngest vs Trigger.dev** : POC ciblé (un workflow de rendu type Video-AI) pour comparer DX, coût, self-host, intégration Next.js. Documenter le choix dans `reference/tools/<outil>.md` et éventuellement un ADR.
- **OpenClaw “assistant auteur”** : oui/non selon envie de l’équipe ; si oui, cadrer permissions et sécurité (voir [OpenClaw Security](https://clawdocs.org/security/overview)).

---

## 5. Références

- [research/inngest](inngest.md) – Détail et verdict Inngest
- [research/trigger-dev](trigger-dev.md) – Détail et verdict Trigger.dev
- [research/mastra](mastra.md) – Rôle agents, complémentarité avec l’orchestrateur
- [research/openclaw](openclaw.md) – Pourquoi pas au cœur du pipeline, option assistant
- [runbooks/video-ai-development](../runbooks/video-ai-development.md) – Procédure v1 (pacing, démos, persistance)
- [packages/skills/thp-video-generation/SKILL.md](../../../packages/skills/thp-video-generation/SKILL.md) – Skill agent : choix visuels, workflow building blocks
- [reference/video-lifecycle](../reference/video-lifecycle.md) – Cycle de vie canonique
- [explanation/video-ai-vision](../explanation/video-ai-vision.md) – Vision v1/v2/v3

