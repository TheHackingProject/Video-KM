# Runbook : Monorepo (Turborepo)

Résumé de l’installation et de l’utilisation du monorepo Video-AI avec Turborepo.

## Stack

- **Turborepo** : orchestration des tâches (build, lint, dev) avec cache et parallélisme
- **Package manager** : Bun
- **Workspaces** : `apps/*`, `packages/*`

## Structure

```
Video-AI/
├── apps/
│   ├── docs/       # App Next.js (docs)
│   └── web/        # App Next.js (web)
├── packages/
│   ├── ui/                 # @repo/ui – composants React partagés
│   ├── eslint-config/      # @repo/eslint-config
│   └── typescript-config/   # @repo/typescript-config
├── turbo.json
├── package.json
└── ...
```

## Installation (nouveau clone)

À la racine du repo :

```bash
# Cloner avec les submodules (KM/Docs, KM/Course/*)
git clone --recurse-submodules <url-du-repo>
cd Video-AI

# Installer les dépendances (Bun)
bun install
```

Si le repo a déjà été cloné sans submodules :

```bash
git submodule update --init --recursive
bun install
```

## Scripts racine

| Script         | Commande              | Rôle                                      |
|----------------|------------------------|-------------------------------------------|
| Build          | `bun run build`        | Build tous les apps/packages (Turbo)      |
| Dev            | `bun run dev`         | Lance les serveurs de dev (Turbo)         |
| Lint           | `bun run lint`        | Lint tous les packages (Turbo)            |
| Check-types    | `bun run check-types` | Vérification TypeScript (Turbo)          |
| Format         | `bun run format`      | Prettier (à migrer vers Biome si besoin)  |

## Utilisation courante

- **Tout builder** : `bun run build` ou `turbo build`
- **Développer une app** : `turbo dev --filter=docs` ou `turbo dev --filter=web`
- **Builder un package** : `turbo build --filter=docs`
- **Lancer tout en dev** : `turbo dev`

## Configuration Turbo (`turbo.json`)

- `build` : dépend de `^build` (build des dépendances d’abord), outputs `.next/**`
- `lint` : dépend de `^lint`
- `check-types` : dépend de `^check-types`
- `dev` : `persistent: true`, `cache: false`

Les tâches sont exécutées dans l’ordre du graphe de dépendances et en cache quand les entrées n’ont pas changé.

## Références

- [Turborepo – Getting started](https://turborepo.dev/docs/getting-started)
- [Turborepo – Add to existing repository](https://turborepo.dev/docs/getting-started/add-to-existing-repository)
