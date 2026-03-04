# projects/

This directory contains one folder per video project. Each folder holds all raw footage, assets, and exported files for that project.

## Naming Convention

Every project folder must follow this pattern:

```
YYYYMMDD_[topic-slug]/
```

- **YYYYMMDD** — the planned publish date (not the creation date)
- **topic-slug** — lowercase words separated by hyphens, no spaces or underscores

**Example:**
```
20260301_building-data-governance/
```

## Folder Structure

Each project folder must contain exactly these sub-folders:

```
YYYYMMDD_[topic-slug]/
├── aroll/                  ← talking head / camera footage
├── screen/                 ← screen recordings
├── assets/
│   ├── canva-exports/      ← exported graphics from Canva
│   └── reference/          ← reference images, inspiration, brand assets
├── export/                 ← final rendered output files
└── _COWORK_INSTRUCTIONS.md
```

## Starting a New Project

1. Copy the `_PROJECT_TEMPLATE/` folder.
2. Rename it using the `YYYYMMDD_[topic-slug]/` convention.
3. Begin adding footage and assets to the appropriate sub-folders.

See `_PROJECT_TEMPLATE/README.md` for full details.

## Special Folders

| Folder | Purpose |
|--------|---------|
| `_PROJECT_TEMPLATE/` | Master template — duplicate and rename for each new project |
