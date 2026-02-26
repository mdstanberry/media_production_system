# AGENTS.md

## Cursor Cloud specific instructions

### Repository overview

This is a **documentation-only repository** for a Media Production System — an AI-augmented solo-creator workflow for video/media content production. As of the initial scaffold, the repo contains:

- Markdown documentation (system design, feature brief, channel configs)
- A project folder structure scaffold with `.gitkeep` placeholders
- Editor configuration (`.editorconfig`) and `.gitignore`
- Cursor rules for media naming conventions (`.cursor/rules/media-naming-conventions.mdc`)

**There is no application code, no dependency manifests (`package.json`, `requirements.txt`, etc.), no tests, and no services to run.** The `apps/`, `scripts/`, `tests/`, `infra/`, and `src/shared/` directories contain only `.gitkeep` files.

### Planned tech stack (not yet implemented)

- **Backend**: Python (FastAPI) / Node.js
- **Notion API scripts**: JavaScript using `@notionhq/client`
- **Database**: Supabase (PostgreSQL)
- **Infrastructure**: GitHub Actions

### Linting

Since the repo is documentation-focused, `markdownlint-cli2` is installed globally for linting:

```sh
markdownlint-cli2 "**/*.md"
```

### Tests and build

No test framework or build system exists yet. When application code is added, update this section with the relevant commands.

### Naming conventions

All media project files and folders follow strict naming conventions defined in `.cursor/rules/media-naming-conventions.mdc`. Review that file before creating or renaming any project folders or export files.
