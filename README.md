# Media Production System

An AI-assisted media production platform managing the full lifecycle of content: ingest, processing, storage, and delivery.

## Project Structure

```
media_production_system/
├── apps/
│   ├── api/        # Backend API service
│   ├── web/        # Frontend web application
│   └── workers/    # Background processing jobs
├── channels/       # Per-platform publishing specs
│   ├── youtube.md
│   ├── linkedin-video.md
│   └── podcast.md
├── docs/
│   ├── architecture/   # System architecture diagrams and notes
│   ├── adrs/           # Architecture Decision Records
│   └── design/         # Design specs and wireframes
├── infra/
│   ├── github-actions/ # CI/CD workflows
│   └── supabase/       # Supabase config and migrations
├── projects/       # One folder per media project (see naming conventions)
│   └── 20260301_building-data-governance/   # Example project
│       ├── aroll/
│       ├── screen/
│       ├── assets/
│       │   ├── canva-exports/
│       │   └── reference/
│       ├── export/
│       └── _COWORK_INSTRUCTIONS.md
├── scripts/        # Dev tooling and automation scripts
├── src/
│   └── shared/     # Shared utilities used across apps
└── tests/          # Cross-component integration tests
```

## File & Folder Naming Conventions

All naming conventions are enforced via `.cursor/rules/media-naming-conventions.mdc`. Key patterns:

| Asset | Pattern |
|-------|---------|
| Project folder | `YYYYMMDD_[topic-slug]/` |
| Final export | `YYYYMMDD_[topic-slug]_v1.mp4` |
| Camtasia project | `YYYYMMDD_[topic-slug]_v1.tscproj` |
| Thumbnail | `YYYYMMDD_[topic-slug]_thumb-A.png` |

## Getting Started

1. Clone the repository
2. See `docs/architecture/` for system overview
3. See individual `apps/` folders for service-specific setup

## Tech Stack

- **Backend**: Python (FastAPI) / Node.js
- **Frontend**: To be defined
- **Database**: Supabase (PostgreSQL)
- **Infrastructure**: GitHub Actions, Supabase
