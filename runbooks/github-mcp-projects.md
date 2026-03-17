---
title: "GitHub MCP Server (toolset projects) – Installation"
type: runbook
diataxis: how-to
status: published
area: tooling
tags:
  - runbook
  - cursor
  - mcp
  - github-projects
created: 2026-03-17
updated: 2026-03-17
related:
  - "[[00-architecture]]"
  - "[[Readme]]"
---

# GitHub MCP Server (toolset projects) – Installation

Connecter Cursor au GitHub Project Video-AI (org TheHackingProject) via le MCP officiel GitHub avec le toolset `projects`. Permet de lister, consulter et mettre à jour les cartes du projet depuis le chat Agent.

---

## 01 – Objectif

- Utiliser les **GitHub Projects v2** (ex. [Video-AI · GitHub](https://github.com/orgs/TheHackingProject/projects/3/views/1)) depuis Cursor.
- Exposer les outils MCP : `projects_list`, `projects_get`, `projects_write` (liste des projets, détail d’un projet, création/mise à jour/déplacement d’items).

---

## 02 – Note sur le toolset projects

Le toolset `projects` n’est **pas** listé dans le README officiel du MCP (qui cite : repos, issues, users, pull_requests, code_security, experiments). Il est disponible depuis la version Docker actuelle (changelog oct 2025, consolidé jan 2026).

- **Image** : `ghcr.io/github/github-mcp-server`.
- Toujours utiliser la dernière image : `docker pull ghcr.io/github/github-mcp-server:latest` — pas de tag fixe, `latest` reçoit les mises à jour du toolset.

---

## 03 – Prérequis

- **Docker** installé et démarré.
- **PAT GitHub** avec :
  - Classic : scope `repo` + `project`
  - Ou fine-grained : org **TheHackingProject**, permission **Projects: Read and write**

---

## 04 – Créer le PAT

1. Sur GitHub : **Settings → Developer settings → Personal access tokens**.
2. **Classic** : cocher `repo` et `project`.
3. **Fine-grained** : sélectionner l’org **TheHackingProject**, puis **Projects: Read and write**.

Conserver le token en lieu sûr ; il ne doit jamais être commité dans le dépôt.

---

## 05 – Configurer le MCP

Créer à la **racine du workspace** le fichier `.cursor/mcp.json` :

**Option A – Token dans le fichier (fichier à ne pas committer)**

```json
{
  "mcpServers": {
    "github": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "-e", "GITHUB_PERSONAL_ACCESS_TOKEN",
        "-e", "GITHUB_TOOLSETS",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "REMPLACER_PAR_TON_PAT",
        "GITHUB_TOOLSETS": "repos,issues,pull_requests,projects"
      }
    }
  }
}
```

Remplacer `REMPLACER_PAR_TON_PAT` par ton PAT réel.

**Option B – Variable d’environnement (à tester selon la version de Cursor)**

Si ta version de Cursor supporte la syntaxe `${env:VAR_NAME}` dans `mcp.json`, tu peux utiliser :

```json
"GITHUB_PERSONAL_ACCESS_TOKEN": "${env:GITHUB_PERSONAL_ACCESS_TOKEN}"
```

Exporter la variable en amont (shell ou secret manager). Dans ce cas, `mcp.json` peut être commité sans secret.

---

## 06 – Sécurité

- Ajouter **immédiatement** dans `.gitignore` :
  - `.cursor/`
  - `.cursor/mcp.json`
- Les deux entrées sont redondantes (`.cursor/` couvre déjà `.cursor/mcp.json`) ; les garder toutes les deux pour clarté documentaire.
- Le double passage du token (`-e GITHUB_PERSONAL_ACCESS_TOKEN` dans les args Docker + variable dans `env` du JSON) est **intentionnel** : la valeur est injectée au process sans être loguée dans l’historique shell.
- Alternatives : variable d’environnement système, envmcp, 1Password CLI, etc., pour éviter le token en clair dans `mcp.json`.

---

## 07 – Installation

1. **Pull de l’image** :
   ```bash
   docker pull ghcr.io/github/github-mcp-server:latest
   ```
   En cas d’erreur d’auth : `docker logout ghcr.io` puis réessayer (l’image est publique).

2. **Redémarrer Cursor** : Cursor relit `mcp.json` au démarrage. Palette de commandes → **MCP: Restart Server**, ou relancer l’application. Vérifier le serveur **github** actif dans les paramètres MCP.

---

## 08 – Vérification

Dans le chat **Agent**, demander :

> Liste les projets GitHub de l’org TheHackingProject

Cursor doit invoquer l’outil `projects_list` via le MCP et afficher les projets (dont Video-AI).

---

## 09 – Outils exposés

- **projects_list** : liste les projets d’un user, d’une org ou d’un repo.
- **projects_get** : récupère un projet avec ses champs et items (détection auto user/org).
- **projects_write** : crée, met à jour, déplace les items (ex. passer une card de To Do → In Progress).

**Limites :**

- `GITHUB_TOOLSETS=all` existe mais est **déconseillé en prod** (trop d’outils → context window saturé) ; garder la liste explicite `repos,issues,pull_requests,projects`.
- Iteration fields (sprints) non couverts nativement.
- Historique de déplacement des items (qui a déplacé quoi, quand) non exposé.
- Status updates (commentaires de progression sur le projet) absents (issue GitHub #1963).

---

## 10 – Rollback

- **Désactiver le MCP** : dans `.cursor/mcp.json`, renommer ou supprimer la clé `github` sous `mcpServers`, puis redémarrer Cursor.
- **Révoquer le PAT** : sur GitHub, Settings → Developer settings → Personal access tokens → supprimer ou révoquer le token utilisé.
