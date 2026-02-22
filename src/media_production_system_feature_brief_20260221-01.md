# Project Feature Brief: Media Production System
**Prepared for:** `create-prd.md` PRD generation process
**Author:** Dean
**Date:** 2026-02-21
**Supporting Reference:** `@youtube_production_system_20260221-01.md`

---

## Problem Statement

A subject matter expert in Facilities Management and Commercial Real Estate needs a
repeatable, re-enterable production system for creating and publishing competent
video and media content across multiple channels — beginning with YouTube.
Currently, no system exists. Each production effort starts from scratch, with no
institutional memory of past ideas, no consistent workflow, and no structured
approach to SEO, scripting, or performance tracking.

The result is inconsistent output quality, duplicated research effort, and
abandoned ideas that are forgotten rather than catalogued for future use.

---

## Goal

Build a structured, AI-augmented Media Production System — version-controlled in
GitHub, documented in Markdown, and operationalized through Notion — that guides
the creator from raw idea through published content and performance analytics,
across any supported media channel.

YouTube is the initial target channel. The architecture must be channel-agnostic
to support future expansion (LinkedIn Video, Podcast, Webinar, etc.) without
requiring a rebuild.

---

## User

- **Name:** Dean
- **Role:** Solo creator / subject matter expert; non-developer on a learning path
- **Tools available:** Cursor, GitHub, Notion, Claude/ChatGPT/Gemini/Perplexity,
  Canva, Camtasia, Make, Cowork (Phase 2)
- **Working style:** Irregular publishing cadence; needs to re-enter any project
  after a gap without losing context

---

## Discrete Buildable Components

The system has three layers. Each is a discrete build scope:

### Layer 1 — Documentation & Configuration (Markdown / GitHub)
Files that define, instruct, and configure the system. No runtime dependencies.

1. **Project folder structure** — standardized directory tree with placeholder
   READMEs per folder
2. **System document** — the full Media Production System reference
   (`docs/media-production-system.md`) — already drafted, needs placement
3. **Prompt template library** — one Markdown file per production stage, containing
   copy-paste LLM prompts with inline customization markers
4. **Cowork instruction templates** — global and folder-specific instruction files
   for Claude Desktop's Cowork agent
5. **Make scenario design spec** — Markdown document specifying the Perplexity API
   → Notion automation logic (input, output, trigger, field mapping)
6. **`.env.example`** — API key placeholder file documenting required credentials
   without exposing values
7. **`.gitignore`** — excluding `.env`, local video project folders, and OS files

### Layer 2 — Notion Infrastructure (Notion API Scripts)
JavaScript scripts using the Notion API (v1) to programmatically create the four
required databases and their schemas.

8. **Idea Registry database** — with all properties per schema: Status, Research
   Notes, Differentiation Angle, Repurposing Tags, Relation to Video Pipeline, etc.
9. **Media Pipeline database** — tracking each project from Scripting through
   Published, with channel property, format branch, and performance metrics fields
10. **Prompt Templates vault** — a Notion page (not database) populated with the
    same prompt library as Layer 1 item 3, formatted for Notion blocks
11. **Performance Insights log** — a Notion page for manually updated pattern
    tracking (high-performing hooks, thumbnails, topics)

### Layer 3 — Automation Specification (Make / Perplexity API)
Design documents and configuration specs — implemented in Make UI, but logic and
field mappings version-controlled in GitHub.

12. **Make scenario spec (detailed)** — step-by-step scenario design for the
    Notion → Perplexity API → Notion research automation, including module
    configuration, field mapping, error handling, and manual fallback procedure

---

## Channel Architecture

The system must treat **channel** as a configurable property, not a hardcoded
assumption. Initial supported channel: **YouTube**.

Channel-specific elements (publishing checklists, SEO field labels, platform
metadata requirements) must be isolated in their own files/database properties so
that adding a new channel (e.g., LinkedIn Video, Podcast) requires adding a new
configuration, not modifying shared infrastructure.

---

## Success Criteria

The build is successful when:

1. The GitHub repository is initialized with the full folder structure and all
   Layer 1 documents in place
2. Running the Notion API scripts creates all four Notion structures with correct
   schemas, property types, and relations — without manual Notion configuration
3. The Make scenario spec is complete and self-contained — a third party could
   build the Make scenario from the document alone, without additional instruction
4. The prompt template library covers all 10 production stages with at least one
   tested, copy-paste-ready prompt per stage
5. A new video project can be initiated end-to-end using only the system — from
   Idea Registry entry through Publishing Checklist — without referencing any
   external documentation

---

## Out of Scope (This Build)

- Actual Make scenario configuration (UI work in Make — done manually post-build)
- Cowork integration (Phase 2 — documented but not activated in this build)
- Any front-end application or web interface
- Automated video file management or cloud storage integration
- Analytics API integrations (YouTube Data API, etc.) — manual entry only in v1
- Channels beyond YouTube (architecture supports them; configuration files are
  not built in this phase)

---

## Technical Constraints

- Notion API: v1 only; use official `@notionhq/client` Node.js SDK
- Scripts must be runnable by a non-developer: clear console output, plain-English
  error messages, no assumed environment beyond Node.js installed
- No paid npm libraries unless unavoidable
- All API credentials via `.env` file; `.env.example` documents required keys
- GitHub repository: private; branch-per-feature workflow per `generate-tasks.md`
- File naming convention for exports:
  `<base_file_name>_YYYYMMDD-NN.ext` (e.g., `media-production-system_20260221-01.md`)

---

## How to Use This Brief

In Cursor, with `create-prd.md` accessible as a rule or referenced file:

```
Use @create-prd.md

Here is the feature I want to build:
@media_production_system_feature_brief_20260221-01.md

Reference this supporting document for full system context:
@youtube_production_system_20260221-01.md
```

The PRD generated from this brief should be saved as:
`/tasks/prd-media-production-system.md`

---

*Media Production System — Project Feature Brief v1.0*
*Input artifact for create-prd.md | Dean | 2026-02-21*
