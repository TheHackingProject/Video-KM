# Runbook : Bun et Biome

Installation et utilisation de Bun (package manager) et Biome (lint + format) dans Video-AI.

---

## Bun (package manager)

### Rôle

- **Package manager** : install des dépendances, scripts, workspaces (comme npm/pnpm/yarn).
- Déclaré dans le repo : `"packageManager": "bun@1.3.10"` dans le `package.json` racine.

### Installation (machine locale)

```bash
# Via curl (voir https://bun.sh)
curl -fsSL https://bun.sh/install | bash
```

### Utilisation dans Video-AI

À la racine du monorepo :

```bash
bun install          # Installer les dépendances
bun run build        # Exécuter le script "build"
bun run dev          # Lancer le dev
bun add -d <pkg>     # Ajouter une devDependency à la racine
```

Les workspaces (`apps/*`, `packages/*`) sont gérés par Bun comme un monorepo classique.

---

## Biome (lint + format)

### Rôle

- **Linter** et **formatter** (remplace ou complète ESLint + Prettier).
- Config dans `biome.json` à la racine du repo Video-AI.
- Version utilisée : `@biomejs/biome` en devDependency (ex. ^2.4.6).

### Installation dans le projet

Déjà installé en devDependency à la racine :

```bash
bun add -d @biomejs/biome
```

Créer ou mettre à jour la config :

```bash
bunx @biomejs/biome init
```

### Important : quelle commande lancer

Sur certaines machines, la commande **`biome`** en terminal pointe vers **un autre outil** (ex. `biom` du paquet `python3-biom-format`), pas vers le linter Biome. On obtient alors un message du type :

```text
Command 'biome' not found, did you mean: command 'biom' from deb python3-biom-format
```

**À utiliser dans Video-AI** (depuis la racine du repo) :

```bash
bunx biome check          # Vérifier format + lint (sans modifier)
bunx biome check --write  # Format + lint + corrections automatiques
bunx biome lint --write   # Lint + correctifs
bunx biome format --write # Format uniquement
```

Ou avec npx si tu n’utilises pas Bun pour la CLI :

```bash
npx @biomejs/biome check --write
```

**À ne pas faire** : compter sur une commande globale `biome` non qualifiée, sauf si tu as explicitement installé le binaire **@biomejs/biome** en global et vérifié que c’est bien lui qui est dans le `PATH`.

### Commandes utiles

| Objectif              | Commande |
|-----------------------|----------|
| Vérifier tout         | `bunx biome check` |
| Corriger + formater   | `bunx biome check --write` |
| Lint seul             | `bunx biome lint` / `bunx biome lint --write` |
| Format seul           | `bunx biome format --write` |
| CI (pas de write)     | `bunx biome ci` |

### Config actuelle (extrait)

- **VCS** : Git, useIgnoreFile
- **Formatter** : activé, indent = tab
- **Linter** : activé, rules recommended
- **JS** : quoteStyle double
- **Assist** : organizeImports on

Fichier complet : `biome.json` à la racine de Video-AI.

### Éditeur (IntelliJ / VS Code)

- **IntelliJ** : plugin officiel “Biome” (Settings → Plugins). Utilise le `biome.json` du projet.
- **VS Code** : extension officielle Biome. Désactiver ESLint/Prettier sur ce repo pour éviter les conflits.

### Migration depuis ESLint / Prettier

- Guide officiel : [Biome – Migrate from ESLint and Prettier](https://biomejs.dev/guides/migrate-eslint-prettier)
- Les commandes `biome migrate eslint` et `biome migrate prettier` sont celles du **CLI Biome** (`@biomejs/biome`). Si `biome` en terminal pointe vers l’autre outil (biom), utiliser explicitement :

  ```bash
  bunx @biomejs/biome migrate eslint
  bunx @biomejs/biome migrate prettier
  ```

---

## Récap

| Outil  | Rôle                    | Commande typique (depuis la racine)     |
|--------|--------------------------|-----------------------------------------|
| Bun    | Install, scripts, workspaces | `bun install`, `bun run build`     |
| Biome  | Lint + format            | `bunx biome check --write`, `bunx biome ci` |

Toujours utiliser **`bunx biome`** (ou **`npx @biomejs/biome`**) pour le linter/formatter Biome, pas la commande globale `biome` non qualifiée.
