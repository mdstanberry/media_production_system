# PRD: Media Production System — Layer 1: Documentation & Configuration

**Project:** Media Production System
**Layer:** 1 of 3
**Author:** Dean
**Date:** 2026-02-21
**Status:** Ready for Development
**Filename:** `prd-layer-1-documentation-config.md`
**Companion PRDs:** `prd-layer-2-notion-infrastructure.md`, `prd-layer-3-automation-spec.md`

---

## 1. Introduction / Overview

This PRD covers the first buildable layer of the Media Production System (MPS): all static documentation, configuration files, and prompt templates that live in the GitHub repository. These files have no runtime dependencies — they define, instruct, and configure the entire system without requiring any API calls or external services.

**The problem they solve:** Dean currently has no persistent production system. Each content project starts from scratch, with no institutional memory, no consistent workflow, and no reusable prompt library. Layer 1 establishes the durable foundation that makes every future production faster, more consistent, and re-enterable after a gap of any length.

**Goal:** Produce a fully populated GitHub repository containing the project structure, the complete system reference document, a pre-populated per-stage prompt library, Cowork instruction templates, a Make scenario design spec, and all required credential and exclusion files.

---

## 2. Goals

1. The GitHub repository contains a standardized folder structure matching the defined project layout — immediately navigable without explanation.
2. The full Media Production System reference document is placed at the correct path and usable on Day 1.
3. Every production stage that uses LLM prompts has a dedicated, pre-populated prompt template file — copy-paste ready, requiring no search through a larger document.
4. All prompt customization variables across every file use the single standardized format `[INSERT: description]` — no exceptions.
5. Cowork instruction templates are authored in full and ready for Phase 2 activation without modification.
6. The Make scenario design spec is complete enough that a third party can build the Make scenario without asking Dean any clarifying questions.
7. The repository is clean: `.env` is excluded from version control, and all required API credential slots are documented in `.env.example`.

---

## 3. User Stories

- As Dean, I want to open a single stage-specific prompt file (e.g., `stage-08-metadata-packaging.md`) mid-production and immediately find the prompts I need — without scanning a large document.
- As Dean, I want every `[INSERT: description]` field in every prompt to follow the same visual pattern, so I can scan quickly for what to fill in before running a prompt.
- As Dean, I want the repository folder structure to already exist with placeholder READMEs, so I know exactly where every artifact from every production stage belongs.
- As Dean, I want to re-enter any video project after a weeks-long gap and find everything I need in the repo and Notion — no reconstruction required.
- As a future Cowork agent or contributor, I want the `_COWORK_INSTRUCTIONS.md` files to provide enough context to execute tasks without asking Dean for clarification.

---

## 4. Functional Requirements

### 4.1 — Repository Folder Structure

1. The repository root must contain the following top-level directories: `docs/`, `channels/`, `prompts/`, `scripts/`, `src/`, `tasks/`, `projects/`.
2. Each directory must contain a `README.md` that explains its purpose and lists its contents (or expected contents).
3. The `projects/` directory must contain a `_PROJECT_TEMPLATE/` subfolder with the standard per-video sub-folder structure:
   ```
   _PROJECT_TEMPLATE/
   ├── aroll/
   ├── screen/
   ├── assets/
   │   ├── canva-exports/
   │   └── reference/
   ├── export/
   └── _COWORK_INSTRUCTIONS.md
   ```
4. The `_PROJECT_TEMPLATE/` subfolder must contain a `README.md` explaining that this folder is duplicated and renamed for each new video project, following the naming convention `YYYYMMDD_[topic-slug]/`.
5. The `channels/` directory must contain: `youtube.md` (active, fully populated) and placeholder files for `linkedin-video.md` and `podcast.md`, each clearly marked `STATUS: Planned — not yet configured`.

### 4.2 — System Document Placement

6. The full Media Production System reference document must be saved verbatim at `docs/media-production-system.md`.
7. The document content must not be modified — it is placed as-is from the existing draft (`src/media_production_system_20260221-01.md`).
8. The `docs/README.md` must identify `media-production-system.md` as the authoritative system reference and describe the other files in the `docs/` directory.

### 4.3 — Prompt Template Library

9. The `prompts/` directory must contain exactly one Markdown file per production stage that uses LLM prompts:

   | File | Stage |
   |---|---|
   | `stage-00-idea-governance.md` | Stage 0 — Idea Governance & Registry |
   | `stage-01-content-strategy.md` | Stage 1 — Ideation & Content Strategy |
   | `stage-02-scripting.md` | Stage 2 — Scripting & Content Structure |
   | `stage-03-pre-production.md` | Stage 3 — Pre-Production Planning (Canva/asset notes only; no LLM prompts — create as a brief operational checklist reference) |
   | `stage-08-metadata-packaging.md` | Stage 8 — Metadata & Packaging |
   | `stage-10-analytics-repurposing.md` | Stage 10 — Analytics & Repurposing |

   > **Note:** Stages 4A, 4B, 4C (recording), 7 (editing), and 9 (publishing checklist) are operational/execution stages with no LLM prompts and do not require prompt template files. Stage 9's YouTube publishing checklist belongs in `channels/youtube.md`.

10. Each prompt file must be pre-populated with all LLM prompts for that stage, extracted directly and verbatim from `docs/media-production-system.md`. No new prompt content is authored.

11. Every prompt file must open with a standard header block containing:
    - Stage number and name
    - One-sentence description of the stage's purpose
    - List of prompts contained in the file
    - When to open this file in the production workflow (e.g., "Open this file when you are ready to evaluate a Raw idea from the Idea Registry.")

12. **Customization variable standard — non-negotiable system requirement:** Every customization variable in every prompt file must use the format `[INSERT: description]` — for example:
    - `[INSERT: working title or topic]`
    - `[INSERT: paste your competitive scan notes from Step B]`
    - `[INSERT: selected hook verbatim]`
    
    No other variable formats are permitted (`[PASTE]`, `<variable>`, bare `[INSERT]`, `{{variable}}`, etc.). This standard enables future automation (Cowork, Make) to parse variables programmatically and makes manual use scannable.

13. Each individual prompt must be preceded by:
    - A level-2 heading with the prompt name (e.g., `## SEO Landscape Scan Prompt`)
    - A "Fill-in checklist" — a bulleted list of every `[INSERT: description]` field the user must complete before running the prompt

14. A `prompts/README.md` must explain the library structure, the `[INSERT: description]` convention, and how to use these files with Cursor Agent (e.g., `@stage-08-metadata-packaging.md`).

15. Each prompt file must end with a `## Performance Notes` section — blank at build time, for Dean to populate as prompts are tested and refined over time.

### 4.4 — Cowork Instruction Templates

16. A global Cowork instruction file must be created at `docs/cowork-global-instructions.md`, containing:
    - Persona: who Dean is and his subject matter expertise (FM/CRE)
    - Audience: FM practitioners and CRE executives
    - Tone: authoritative, explanatory, conversational
    - Output format defaults: speaking bullets (not prose), flag jargon for plain-language alternatives
    - Status marker at the top: `STATUS: Phase 2 — Not yet activated`

17. The `projects/_PROJECT_TEMPLATE/_COWORK_INSTRUCTIONS.md` must contain folder-specific Cowork instructions covering the three defined use cases:
    - **Use Case 1 — Script Assembly** (triggers after Stage 1 Content Brief is complete)
    - **Use Case 2 — SEO Package Assembly** (triggers after video export, with transcript in folder)
    - **Use Case 3 — Repurposing Package** (triggers after publishing)
    
    Each use case must specify: trigger condition, folder contents Cowork should read, and the exact output requested.

18. Both Cowork files must carry the status marker: `STATUS: Phase 2 — Not yet activated. Do not modify shared infrastructure to enable this integration.`

### 4.5 — Make Scenario Design Spec

19. A Make scenario design spec must be created at `docs/make-scenario-spec.md`.

    > **Note:** This file is the primary deliverable for Layer 3. Its full functional requirements are defined in `prd-layer-3-automation-spec.md`. The Layer 1 requirement is simply that this file exists at the correct path in the repository as part of the Layer 1 build. Content requirements are governed by the Layer 3 PRD.

### 4.6 — `.env.example`

20. An `.env.example` file must exist at the repository root.
21. It must document every required API credential with a plain-English inline comment explaining what the key is for and where to obtain it:
    ```
    # Notion Integration Token — obtain from https://www.notion.so/my-integrations
    NOTION_API_KEY=

    # Perplexity API Key — obtain from https://www.perplexity.ai/settings/api
    PERPLEXITY_API_KEY=
    ```
22. All values must be empty placeholders. No real credentials may appear in this file.

### 4.7 — `.gitignore`

23. The `.gitignore` must exist at the repository root and must exclude the following, each with an inline comment:
    - `.env` and `*.env` — contains real API credentials; never commit
    - `node_modules/` — npm dependency folder; reinstall with `npm install`
    - `Thumbs.db`, `.DS_Store` — OS-generated files with no project value
    - `projects/[0-9]*/` — local video project folders; video files are too large for Git and are managed locally

---

## 5. Non-Goals (Out of Scope for This Layer)

- **Writing new prompt content.** All prompts are extracted from the existing system document. No new prompts are authored in Layer 1.
- **Configuring or activating Cowork.** Instruction files are created but Cowork integration is Phase 2.
- **Building or configuring the Make scenario in the Make UI.** The spec document only (content governed by Layer 3 PRD).
- **Creating Notion databases or pages.** That is Layer 2.
- **Channels beyond YouTube.** `linkedin-video.md` and `podcast.md` are stub placeholder files only.
- **Any front-end application, web interface, or runtime code.**

---

## 6. Design Considerations

- **One prompt file per stage** is a deliberate design decision — not a preference. It mirrors how the system is actually used: a creator opens the file for the stage they're in, not the full library. It also makes Cursor Agent references unambiguous: `@stage-08-metadata-packaging.md` is precise; `@prompt-library.md` requires the agent to search for the right section every time.

- **`[INSERT: description]` standardization** is a system-wide convention, not a style preference. Consistent markers enable future automation (Cowork, Make) to parse variables programmatically. Every prompt in every file must conform — no legacy formats carried over from the system document.

- **Cowork files authored in full, activated later.** The instruction content should be complete and correct in Layer 1 so that Phase 2 activation is a configuration step, not an authoring step.

- **`channels/youtube.md` is the YouTube publishing checklist home.** The Stage 9 publishing checklist from the system document belongs here, not in a prompt template file. This isolates channel-specific content so adding a new channel means adding a new file, not modifying shared infrastructure.

---

## 7. Technical Considerations

- No runtime dependencies in Layer 1 — all files are static Markdown and plain text.
- Repository is private on GitHub; branch-per-feature workflow applies per project conventions.
- The system document (`docs/media-production-system.md`) is the single source of truth for all prompt content. Prompt template files are copies, not rewrites. If a prompt appears in both the system doc and a template file, they must be identical.
- File naming convention for this repository (not video project exports): lowercase, words separated by hyphens, descriptive (e.g., `make-scenario-spec.md`, `stage-08-metadata-packaging.md`).
- Video project export file naming convention (applies inside `projects/` subdirectories): `YYYYMMDD_[topic-slug]_v1.ext`.

---

## 8. Success Metrics

1. All 7 Layer 1 components exist in the repository at the correct paths.
2. Every prompt template file opens directly to usable, copy-paste-ready prompts — no missing content, no placeholder sections (except `## Performance Notes`).
3. A reviewer can open any prompt file, immediately identify all `[INSERT: description]` fields by visual scan, and run the prompt without referring to any other document.
4. Zero instances of non-standard variable formats (`[PASTE]`, `<variable>`, bare `[INSERT]`) exist across all prompt files.
5. The Make scenario spec is reviewed by Dean and confirmed self-contained — no clarifying questions needed to build the scenario.
6. Running `git status` on a fresh clone shows no untracked sensitive files (`.env` correctly excluded).
7. Dean can navigate to any stage's prompt file in under 10 seconds from the repository root.

---

## 9. Open Questions

- **`channels/youtube.md` scope:** Should this file contain only the publishing checklist (Stage 9), or also the YouTube-specific SEO field labels and platform metadata requirements referenced in the system architecture overview? Recommendation: include all YouTube-specific elements — make the channel file fully self-contained.
- **`stage-03-pre-production.md` treatment:** Stage 3 has no LLM prompts — it's operational checklists. Should it be included in the `prompts/` library as a checklist reference, or moved to `docs/`? Recommendation: include in `prompts/` as an operational reference file, clearly noted as "no LLM prompts — production checklist only," so the library covers all active stages in one place.

---

*Media Production System — Layer 1 PRD v1.0*
*Input: `media_production_system_feature_brief_20260221-01.md` + clarifying answers | Dean | 2026-02-21*
