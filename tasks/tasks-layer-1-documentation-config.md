## Relevant Files

- `docs/media_production_system_20260221-01.md` — Source document; all prompt content must be copied verbatim from here.
- `docs/media-production-system.md` — Authoritative system reference; created by copying the source doc to the correct path and name.
- `docs/README.md` — Identifies `media-production-system.md` as the system reference and describes the docs/ directory.
- `docs/cowork-global-instructions.md` — Global Cowork persona, audience, and output defaults (Phase 2 activation file).
- `docs/make-scenario-spec.md` — Make scenario design spec stub (content governed by Layer 3 PRD).
- `channels/youtube.md` — Active YouTube config; must include publishing checklist and YouTube-specific SEO fields.
- `channels/linkedin-video.md` — Planned channel stub; must carry correct status marker.
- `channels/podcast.md` — Planned channel stub; must carry correct status marker.
- `prompts/README.md` — Explains the prompt library structure, `[INSERT: description]` convention, and Cursor Agent usage.
- `prompts/stage-00-idea-governance.md` — Stage 0 prompt template file.
- `prompts/stage-01-content-strategy.md` — Stage 1 prompt template file.
- `prompts/stage-02-scripting.md` — Stage 2 prompt template file.
- `prompts/stage-03-pre-production.md` — Stage 3 operational checklist (no LLM prompts).
- `prompts/stage-08-metadata-packaging.md` — Stage 8 prompt template file.
- `prompts/stage-10-analytics-repurposing.md` — Stage 10 prompt template file.
- `projects/README.md` — Explains the projects/ directory and the YYYYMMDD_[topic-slug]/ naming convention.
- `projects/_PROJECT_TEMPLATE/README.md` — Explains this folder is duplicated and renamed for each new video project.
- `projects/_PROJECT_TEMPLATE/_COWORK_INSTRUCTIONS.md` — Per-project Cowork instructions with all 3 use cases.
- `.env.example` — Documents all required API credential slots with inline comments; all values empty.
- `.gitignore` — Updated to include all required exclusions per PRD with inline comments.

### Notes

- All files in this layer are static Markdown or plain text — no runtime code, no tests required.
- Every file must follow the repo naming convention: lowercase, hyphen-separated (e.g., `stage-08-metadata-packaging.md`).
- The system document (`docs/media-production-system.md`) is the single source of truth for all prompt content. Copy prompts verbatim — do not paraphrase or rewrite.
- Every customization variable in every prompt file must use `[INSERT: description]` format — no other formats permitted (`[PASTE]`, `<variable>`, bare `[INSERT]`, `{{variable}}`).

## Instructions for Completing Tasks

**IMPORTANT:** As you complete each task, check it off by changing `- [ ]` to `- [x]`. Update after each sub-task, not just after the parent task.

Example:
- `- [ ] 1.1 Create docs/ folder` → `- [x] 1.1 Create docs/ folder` (after completing)

## Tasks

- [x] 0.0 Create feature branch
  - [x] 0.1 Create and checkout a new branch: `git checkout -b feature/layer-1-documentation-config`

- [x] 1.0 Set up repository folder structure and README files
  - [x] 1.1 Create the `prompts/` directory at the repo root (the only required top-level directory currently missing)
  - [x] 1.2 Create `docs/README.md` — describe the docs/ directory purpose; note that `media-production-system.md` is the authoritative system reference and list all other files in the directory
  - [x] 1.3 Create `channels/README.md` — describe the channels/ directory purpose and list the three channel files with their current status
  - [x] 1.4 Create `projects/README.md` — explain the projects/ directory purpose and document the `YYYYMMDD_[topic-slug]/` naming convention
  - [x] 1.5 Create `scripts/README.md` — describe the scripts/ directory purpose (placeholder for future automation scripts)
  - [x] 1.6 Create `tasks/README.md` — describe the tasks/ directory purpose (PRD and task list files for each system layer)
  - [x] 1.7 Create `src/README.md` — describe the src/ directory purpose (source/draft documents)
  - [x] 1.8 Create the `projects/_PROJECT_TEMPLATE/` folder structure with all required sub-folders: `aroll/`, `screen/`, `assets/canva-exports/`, `assets/reference/`, `export/` (use `.gitkeep` files to preserve empty folders in Git)
  - [x] 1.9 Create `projects/_PROJECT_TEMPLATE/README.md` — explain that this folder is the master template: duplicate and rename it to `YYYYMMDD_[topic-slug]/` for each new video project

- [x] 2.0 Place system reference document at correct path
  - [x] 2.1 Copy `docs/media_production_system_20260221-01.md` to `docs/media-production-system.md` — content must be verbatim, no edits
  - [x] 2.2 Verify the copied file opens correctly and content matches the source exactly

- [x] 3.0 Build prompt template library (6 stage files + README)
  - [x] 3.1 Open `docs/media-production-system.md` and locate all LLM prompts for each of the six stages (0, 1, 2, 3, 8, 10); note which sections correspond to each stage before writing any files
  - [x] 3.2 Create `prompts/stage-00-idea-governance.md` — standard header block (stage number/name, one-sentence description, list of prompts, when to open); all prompts verbatim from the system doc; fill-in checklist before each prompt; `## Performance Notes` section at end (blank)
  - [x] 3.3 Create `prompts/stage-01-content-strategy.md` — same structure as 3.2
  - [x] 3.4 Create `prompts/stage-02-scripting.md` — same structure as 3.2
  - [x] 3.5 Create `prompts/stage-03-pre-production.md` — header block noting "no LLM prompts — production checklist only"; include the Stage 3 operational checklist content from the system doc; no fill-in checklist needed; `## Performance Notes` section at end
  - [x] 3.6 Create `prompts/stage-08-metadata-packaging.md` — same structure as 3.2
  - [x] 3.7 Create `prompts/stage-10-analytics-repurposing.md` — same structure as 3.2
  - [x] 3.8 Create `prompts/README.md` — explain the library structure, the `[INSERT: description]` convention, and how to reference files in Cursor Agent (e.g., `@stage-08-metadata-packaging.md`)
  - [x] 3.9 Scan all six prompt files for non-standard variable formats (`[PASTE]`, `<variable>`, bare `[INSERT]` without a description, `{{variable}}`); correct any found to `[INSERT: description]`

- [x] 4.0 Create Cowork instruction template files
  - [x] 4.1 Create `docs/cowork-global-instructions.md` containing: status marker at top (`STATUS: Phase 2 — Not yet activated. Do not modify shared infrastructure to enable this integration.`), Dean's persona and subject matter expertise (FM/CRE), target audience (FM practitioners and CRE executives), tone guidelines (authoritative, explanatory, conversational), and output format defaults (speaking bullets not prose; flag jargon for plain-language alternatives)
  - [x] 4.2 Populate `projects/_PROJECT_TEMPLATE/_COWORK_INSTRUCTIONS.md` with the same status marker at top, then the three use cases in full detail:
    - Use Case 1 — Script Assembly: trigger condition, folder contents to read, exact output requested
    - Use Case 2 — SEO Package Assembly: trigger condition, folder contents to read, exact output requested
    - Use Case 3 — Repurposing Package: trigger condition, folder contents to read, exact output requested
  - [x] 4.3 Verify both Cowork files carry the identical status marker: `STATUS: Phase 2 — Not yet activated. Do not modify shared infrastructure to enable this integration.`

- [x] 5.0 Create Make scenario design spec stub
  - [x] 5.1 Create `docs/make-scenario-spec.md` — placeholder file at the correct path with a header, a note that content is governed by `prd-layer-3-automation-spec.md`, and blank section headings ready to be filled in during Layer 3 build

- [x] 6.0 Create `.env.example` and update `.gitignore`
  - [x] 6.1 Create `.env.example` at the repo root with two credential slots (`NOTION_API_KEY=` and `PERPLEXITY_API_KEY=`), each preceded by an inline comment explaining what the key is for and where to obtain it; all values must be empty
  - [x] 6.2 Review the existing `.gitignore` and add the following entries if not already present, each with an inline comment:
    - `*.env` — catches any environment file variant
    - `projects/[0-9]*/` — local video project folders (video files too large for Git, managed locally)
    - Verify `Thumbs.db` and `.DS_Store` are already present (they are; confirm and leave as-is)
  - [x] 6.3 Do a final check: confirm `.env` is excluded by running `git check-ignore -v .env` in the terminal — expected output should show `.gitignore` as the matching rule
