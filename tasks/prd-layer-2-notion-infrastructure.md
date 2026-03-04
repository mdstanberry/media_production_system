# PRD: Media Production System — Layer 2: Notion Infrastructure

**Project:** Media Production System
**Layer:** 2 of 3
**Author:** Dean
**Date:** 2026-02-21
**Status:** Ready for Development
**Filename:** `prd-layer-2-notion-infrastructure.md`
**Companion PRDs:** `prd-layer-1-documentation-config.md`, `prd-layer-3-automation-spec.md`

---

## 1. Introduction / Overview

This PRD covers the second buildable layer of the Media Production System: the four Notion structures — two databases and two pages — that serve as the production memory for the entire system. These structures are built directly by Cursor using its Notion MCP integration, targeting the existing "Media Production System" Notion root page. No manual Notion configuration is required.

**The problem they solve:** Without structured Notion databases, ideas are lost, projects lose context between sessions, and there is no institutional memory of what worked and what didn't. Layer 2 transforms the existing Notion page into a fully operational production system.

**Notion root page:** Media Production System
**Notion page ID:** `30f5b56017ff80a8a952d887c6fe9a7a`
**Build method:** Cursor Notion MCP (direct creation — no Node.js scripts required for execution)

---

## 2. Goals

1. All four Notion structures are created inside the "Media Production System" root page with correct schemas, property types, and relations — without any manual Notion configuration by Dean.
2. The Idea Registry and Media Pipeline databases are linked via a two-way Relation property so ideas and projects are navigable in both directions.
3. The Prompt Templates Vault is organized by stage and contains all prompts from the system document, formatted as Notion toggle blocks.
4. The Performance Insights Log is structured with blank tables ready for manual entry from Day 1.
5. A navigation block on the root page links to all four structures.

---

## 3. User Stories

- As Dean, I want to log a new idea in Notion with a working title, source, and date captured, and immediately see it in a Board view organized by status.
- As Dean, I want to change an idea's status to "Research Requested" to trigger the Make automation (Layer 3), with the correct status option already present in the database.
- As Dean, I want to track every video project from Scripting through Published in a single database, with all relevant links and performance metrics in one place.
- As Dean, I want to open the Prompt Templates Vault in Notion and find the right prompt for any production stage — organized by stage, expandable on demand — without leaving Notion.
- As Dean, I want to log performance patterns (hooks, thumbnails, topics) in a persistent structured log so I can reference them when planning future videos.

---

## 4. Functional Requirements

### 4.1 — Idea Registry Database (N1)

1. Create a Notion database titled **"Idea Registry"** as a direct child of the Media Production System root page (ID: `30f5b56017ff80a8a952d887c6fe9a7a`).

2. The database must include the following properties with the specified types and configurations:

   | Property Name | Property Type | Configuration |
   |---|---|---|
   | Idea Title | Title | Default title field |
   | Status | Select | Options (in order): Raw, Research Requested, Research Complete, Validated, Needs Refinement, Abandoned, In Production, Published |
   | Target Channel | Select | Options: YouTube, LinkedIn Video, Podcast, Webinar, Other |
   | Date Captured | Date | — |
   | Source / Trigger | Rich Text | — |
   | SEO Research Notes | Rich Text | — |
   | Differentiation Angle | Rich Text | — |
   | Decision Rationale | Rich Text | — |
   | Repurposing Tags | Multi-select | Options: Blog/Article, LinkedIn Post, Newsletter, YouTube Short, Slide Deck, Podcast/Audio |
   | Linked Media Project | Relation | Two-way relation → Media Pipeline database |
   | Date Decided | Date | — |
   | Competing Content Found | URL | — |

3. Default view: **Board view** grouped by the `Status` property — all status columns visible (including Abandoned; abandoned ideas are institutional memory and must remain visible).

4. Second view: **Table view** showing all properties.

5. The database must include a brief description (Notion database description field): *"Every idea captured here. Volume at capture; judgment at the Decision Gate."*

### 4.2 — Media Pipeline Database (N2)

6. Create a Notion database titled **"Media Pipeline"** as a direct child of the Media Production System root page.

7. The database must include the following properties:

   | Property Name | Property Type | Configuration |
   |---|---|---|
   | Project Title | Title | Default title field |
   | Status | Select | Options (in order): Scripting, Pre-Production, Recording, Editing, Packaging, Ready to Publish, Published |
   | Target Channel | Select | Options: YouTube, LinkedIn Video, Podcast, Webinar, Other |
   | Format Branch | Select | Options: A — Talking Head + Screen, B — Green Screen, C — Slide Voiceover |
   | Target Length | Number | Format: Number (minutes) |
   | Content Brief | URL | — |
   | Performance Script | URL | — |
   | Camtasia Project File | Rich Text | File path / naming reference |
   | Export File | Rich Text | File path / naming reference |
   | Publish Date | Date | — |
   | Published URL | URL | — |
   | 7-Day CTR | Number | Format: Percent |
   | 7-Day Avg Duration | Number | Format: Number (seconds) |
   | 30-Day Views | Number | — |
   | 30-Day Subscribers | Number | — |
   | Notes | Rich Text | — |
   | Linked Idea | Relation | Two-way relation → Idea Registry database (back-relation to Req. 2 above) |

8. The `Linked Idea` ↔ `Linked Media Project` relation must be configured as a **two-way (synced) relation** so that linking a Media Pipeline entry to an Idea Registry entry automatically creates the reverse link.

9. Default view: **Board view** grouped by the `Status` property.

10. Second view: **Table view** showing all properties.

11. The database must include a brief description: *"One entry per video project, from first script through published performance data."*

### 4.3 — Prompt Templates Vault (N3)

12. Create a Notion page titled **"Prompt Templates Vault"** as a direct child of the Media Production System root page.

13. The page must open with a one-paragraph introduction explaining: what this vault is, how to use it (find your stage, expand the toggle, copy the prompt, fill in all `[INSERT: description]` fields before running), and a note that the canonical source is the GitHub prompt library (`prompts/` directory).

14. The page must be organized into sections by production stage, using Notion Heading 2 for each stage header:
    - Stage 0 — Idea Governance & Registry
    - Stage 1 — Ideation & Content Strategy
    - Stage 2 — Scripting & Content Structure
    - Stage 8 — Metadata & Packaging
    - Stage 10 — Analytics & Repurposing

15. Each stage section must open with a one-sentence description of when to use the prompts in that section.

16. Each individual prompt must be formatted as a **Notion toggle block**:
    - Toggle title: prompt name (e.g., "SEO Landscape Scan Prompt")
    - Toggle body: full prompt text, verbatim from the system document

17. All customization variables within prompt text must use the standardized `[INSERT: description]` format — no other variable formats permitted.

18. Each stage section must end with a **"Performance Notes"** toggle block — empty at build time, for Dean to populate as prompts are tested and refined.

### 4.4 — Performance Insights Log (N4)

19. Create a Notion page titled **"Performance Insights Log"** as a direct child of the Media Production System root page.

20. The page must open with a brief instruction: *"Update this log after reviewing 7-day and 30-day analytics for each published video. This is your institutional memory for what works."*

21. The page must contain the following five sections, each as a Notion Heading 2 with a structured table beneath it:

    **Hooks That Worked**
    | Hook Text | Video Title | CTR % | Notes |
    |---|---|---|---|
    | — | — | — | — |

    **Thumbnail Patterns That Worked**
    | Visual Style Description | CTR % | Notes |
    |---|---|---|
    | — | — | — |

    **Topics That Performed**
    | Topic / Angle | Views at 30 Days | Notes |
    |---|---|---|
    | — | — | — |

    **Topics That Underperformed**
    | Topic / Angle | Likely Reason | Notes |
    |---|---|---|
    | — | — | — |

    **Prompt Refinements**
    | Prompt Name | What Changed | Date | Result |
    |---|---|---|---|
    | — | — | — | — |

22. Each section heading must include a brief instruction note (Notion callout block) explaining what to log in that section.

### 4.5 — Root Page Navigation

23. The "Media Production System" root page must be updated to include a navigation block at the top (before any existing content) that links to all four structures:
    - Idea Registry (database)
    - Media Pipeline (database)
    - Prompt Templates Vault (page)
    - Performance Insights Log (page)

24. The navigation block must also include a link to the GitHub repository (placeholder: `[GitHub Repository — add URL after setup]`).

---

## 5. Non-Goals (Out of Scope for This Layer)

- **Configuring the Make webhook trigger.** The `Research Requested` status option is created here, but the Make automation that watches for it is built in Layer 3 / Make UI.
- **Populating the Idea Registry or Media Pipeline with real project data.** Databases are created empty.
- **Building Notion templates for individual video project pages.** Not required in v1.
- **Formula, rollup, or computed properties** beyond what is specified above.
- **Filtered views** (e.g., "YouTube Only") — deferrable once real data exists.
- **Any Notion content beyond the four structures** listed in this PRD.

---

## 6. Technical Considerations

- **Build method:** All four structures are created via Cursor's Notion MCP integration in a single session. No Node.js scripts are required.
- **Audit record:** After build, a `scripts/notion-build-log.md` file should be created in the repository documenting what was built (structure names, IDs, creation date) for recovery and replication purposes.
- **Parent page ID:** `30f5b56017ff80a8a952d887c6fe9a7a` — all structures must be direct children of this page.
- **Relation configuration:** The two-way relation between Idea Registry and Media Pipeline is a single relation configured once; Notion automatically creates the back-property. Confirm both properties appear in both databases after creation.
- **Notion API version:** v1 (relevant if scripts are later needed for replication on a new workspace).
- **Make trigger compatibility:** The `Research Requested` status option in the Idea Registry must match the exact string used in the Make scenario trigger condition (see `prd-layer-3-automation-spec.md`). Do not rename this option after build.

---

## 7. Success Metrics

1. All four structures exist as direct children of the Media Production System Notion root page.
2. The Idea Registry and Media Pipeline databases are linked bidirectionally — adding a relation in one database automatically populates the other.
3. Dean can create a new Idea Registry entry, set Status to "Research Requested," and confirm the option is present and correctly named.
4. The Prompt Templates Vault contains all prompts organized by stage, each in a toggle block with `[INSERT: description]` variables throughout.
5. The Performance Insights Log contains all five sections with blank tables ready for manual entry.
6. The root page navigation block links correctly to all four structures.
7. Dean can complete a full Stage 0 idea capture → decision workflow entirely within Notion, without referencing any external document.

---

## 8. Open Questions

- **Idea Registry Board view — should Abandoned be visible by default?** Recommendation: yes — abandoned ideas are institutional memory. A separate filtered view hiding Abandoned can be added later if the board becomes cluttered.
- **Should filtered views per channel be created now?** Recommendation: no — defer until real data exists. A "YouTube Only" filter on an empty database adds no value at build time.
- **Root page existing content:** The root Notion page may already have content. Confirm with Dean before modifying it. The navigation block should be added at the top without removing existing content.

---

*Media Production System — Layer 2 PRD v1.0*
*Input: `media_production_system_feature_brief_20260221-01.md` + clarifying answers | Dean | 2026-02-21*
*Notion parent page ID: `30f5b56017ff80a8a952d887c6fe9a7a`*
