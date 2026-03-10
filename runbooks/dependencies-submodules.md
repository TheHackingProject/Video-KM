# Runbook : Dépendances et dépôts GitHub (submodules)

Comment intégrer d’autres dépôts GitHub dans Video-AI via Git submodules.

## Principe

Un **submodule** est un dépôt Git cloné dans un sous-dossier du repo. Le repo parent enregistre uniquement l’**URL** et le **commit** du submodule (pas les fichiers). Chaque submodule reste un repo Git indépendant.

## Submodules actuels (Video-AI)

Définis dans `.gitmodules` à la racine :

| Chemin            | URL (dépôt GitHub)                              |
|-------------------|--------------------------------------------------|
| `KM/Docs`         | https://github.com/TheHackingProject/Video-KM.git |
| `KM/Course/Intro` | https://github.com/TheHackingProject/course-intro.git |
| `KM/Course/Fullstack` | https://github.com/TheHackingProject/course-fullstack.git |
| `KM/Course/React` | https://github.com/TheHackingProject/next-react.git |

## Ajouter un nouveau submodule

À la **racine** du repo Video-AI, avec un **chemin relatif** et l’URL en `.git` :

```bash
cd /chemin/vers/Video-AI
git submodule add https://github.com/org/repo.git KM/NomDuDossier
```

Exemple (Video-KM dans `KM/Docs`) :

```bash
git submodule add https://github.com/TheHackingProject/Video-KM.git KM/Docs
```

- Le dossier cible ne doit **pas exister** ou doit être **vide**.
- Utiliser un chemin **relatif** (ex. `KM/Docs`) pour que le repo reste portable.

## Cloner le repo avec les submodules

```bash
git clone --recurse-submodules https://github.com/.../Video-AI.git
cd Video-AI
```

Si le repo a déjà été cloné **sans** submodules :

```bash
git submodule update --init --recursive
```

## Mettre à jour un submodule

Entrer dans le dossier du submodule, puis utiliser Git normalement :

```bash
cd KM/Docs
git fetch origin
git checkout main
git pull
cd ../..
git add KM/Docs
git commit -m "chore: update submodule KM/Docs"
```

Enregistrer le nouveau commit dans le repo parent (comme ci-dessus) pour que l’équipe et la CI utilisent la même version.

## Comprendre un diff "Subproject commit"

Un diff sur un submodule affiche uniquement le **changement de commit** pointé :

```
-Subproject commit 28e1c0e...
+Subproject commit 67b8acf...
```

C’est normal : le parent ne versionne pas le contenu du submodule, seulement la référence au commit.

## Commandes utiles

| Action                    | Commande |
|---------------------------|----------|
| Ajouter un submodule      | `git submodule add <url> <path>` |
| Clone avec submodules     | `git clone --recurse-submodules <url>` |
| Init/update submodules    | `git submodule update --init --recursive` |
| Statut des submodules     | `git submodule status` |
| Mettre à jour tous        | `git submodule update --remote --merge` |

## Intégration avec le monorepo

- Les submodules sous `KM/` sont des **contenus/docs** (Video-KM, cours). Ils ne sont pas des workspaces Bun (`apps/*`, `packages/*`).
- Pour utiliser un dépôt externe comme **package** du monorepo : l’ajouter en submodule dans `packages/` (avec un `package.json`), puis l’inclure dans les workspaces si besoin, ou le référencer via `"dep": "file:./packages/..."` dans les apps.

## Références

- [Git – Submodules](https://git-scm.com/book/fr/v2/Utilitaires-Git-Sous-modules)
- [Turborepo – Add to existing repository](https://turborepo.dev/docs/getting-started/add-to-existing-repository) (workspaces vs contenu externe)
