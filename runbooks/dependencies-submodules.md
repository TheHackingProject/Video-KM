---
title: "Dependencies and GitHub Repositories (Submodules) Runbook"
type: runbook
status: published
area: development
tags:
  - documentation
  - runbook
  - git
  - submodules
  - dependencies
created: 2026-03-10
updated: 2026-03-11
related:
  - "[[00-architecture]]"
  - "[[01-index]]"
  - "[[runbooks/monorepo]]"
---

# Runbook: Dependencies and GitHub Repositories (Submodules)

How to integrate other GitHub repositories into Video-AI via Git submodules.

## Principle

A **submodule** is a Git repository cloned into a subdirectory of the parent repo. The parent stores only the submodule **URL** and **commit** (not its files). Each submodule remains an independent Git repository.

## Current submodules (Video-AI)

Defined in `.gitmodules` at the root:

| Path                  | GitHub URL                                              |
|-----------------------|---------------------------------------------------------|
| `KM/Docs`             | https://github.com/TheHackingProject/Video-KM.git       |
| `KM/Course/Intro`     | https://github.com/TheHackingProject/course-intro.git   |
| `KM/Course/Fullstack` | https://github.com/TheHackingProject/course-fullstack.git |
| `KM/Course/React`     | https://github.com/TheHackingProject/next-react.git     |
| `packages/skills/Remotion` | https://github.com/remotion-dev/skills.git         |

## Adding a new submodule

From the **root** of the Video-AI repo, using a **relative path** and URL with `.git`:

```bash
cd /path/to/Video-AI
git submodule add https://github.com/org/repo.git KM/FolderName
```

Example (Video-KM in `KM/Docs`):

```bash
git submodule add https://github.com/TheHackingProject/Video-KM.git KM/Docs
```

- The target folder must **not exist** or must be **empty**.
- Use a **relative path** (e.g. `KM/Docs`) so the repo stays portable.

## Cloning the repo with submodules

```bash
git clone --recurse-submodules https://github.com/.../Video-AI.git
cd Video-AI
```

If the repo was already cloned **without** submodules:

```bash
git submodule update --init --recursive
```

## Updating a submodule

Enter the submodule directory, then use Git as usual:

```bash
cd KM/Docs
git fetch origin
git checkout main
git pull
cd ../..
git add KM/Docs
git commit -m "chore: update submodule KM/Docs"
```

Record the new commit in the parent repo (as above) so the team and CI use the same version.

## Understanding a "Subproject commit" diff

A diff on a submodule shows only the **commit reference change**:

```
-Subproject commit 28e1c0e...
+Subproject commit 67b8acf...
```

This is expected: the parent does not version submodule content, only the commit reference.

## Useful commands

| Action                 | Command                                      |
|------------------------|----------------------------------------------|
| Add a submodule        | `git submodule add <url> <path>`             |
| Clone with submodules  | `git clone --recurse-submodules <url>`       |
| Init/update submodules | `git submodule update --init --recursive`    |
| Submodule status       | `git submodule status`                       |
| Update all             | `git submodule update --remote --merge`      |

## Integration with the monorepo

- Submodules under `KM/` are **content/docs** (Video-KM, courses). They are not Bun workspaces (`apps/*`, `packages/*`).
- To use an external repo as a **package** in the monorepo: add it as a submodule under `packages/` (with a `package.json`), then include it in workspaces if needed, or reference it via `"dep": "file:./packages/..."` in apps.

## References

- [Git – Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
- [Turborepo – Add to existing repository](https://turborepo.dev/docs/getting-started/add-to-existing-repository) (workspaces vs external content)
