## Relevant Files

- `tasks/prd-layer-2-notion-infrastructure.md` - The PRD this task list implements. Reference for all property schemas, view requirements, and descriptions.
- `docs/media_production_system_20260221-01.md` - Source document containing all prompt text to copy verbatim into the Prompt Templates Vault (N3).
- `scripts/notion-build-log.md` - To be created in Task 7.0. Records all built Notion structure names, IDs, and creation date for recovery and replication.

### Notes

- All Notion structures are built via Cursor's Notion MCP integration — no Node.js scripts required.
- Parent Notion page ID: `30f5b56017ff80a8a952d887c6fe9a7a` (Media Production System root page).
- Always read the Notion MCP tool schema (in `mcps/plugin-notion-workspace-notion/tools/`) before calling any MCP tool, to confirm required parameters.
- The `Research Requested` status option in the Idea Registry must match this exact string — do not rename it after creation (it is the Make automation trigger in Layer 3).
- After the two-way relation is configured (Task 3.0), confirm that both back-properties appear in both databases before proceeding.

## Instructions for Completing Tasks

**IMPORTANT:** As you complete each task, you must check it off in this markdown file by changing `- [ ]` to `- [x]`. This helps track progress and ensures you don't skip any steps.

Example:
- `- [ ] 1.1 Read file` → `- [x] 1.1 Read file` (after completing)

Update the file after completing each sub-task, not just after completing an entire parent task.

## Tasks

- [ ] 0.0 Create feature branch
  - [ ] 0.1 Create and checkout a new branch for this build (e.g., `git checkout -b feature/layer-2-notion-infrastructure`)

- [ ] 1.0 Create Idea Registry Database (N1)
  - [ ] 1.1 Read the Notion MCP tool schemas in `mcps/plugin-notion-workspace-notion/tools/` to confirm the correct parameters for creating a database and its properties
  - [ ] 1.2 Create the "Idea Registry" database as a direct child of the root page (ID: `30f5b56017ff80a8a952d887c6fe9a7a`) with the following properties and types:
    - `Idea Title` — Title (default title field)
    - `Status` — Select with options in order: Raw, Research Requested, Research Complete, Validated, Needs Refinement, Abandoned, In Production, Published
    - `Target Channel` — Select with options: YouTube, LinkedIn Video, Podcast, Webinar, Other
    - `Date Captured` — Date
    - `Source / Trigger` — Rich Text
    - `SEO Research Notes` — Rich Text
    - `Differentiation Angle` — Rich Text
    - `Decision Rationale` — Rich Text
    - `Repurposing Tags` — Multi-select with options: Blog/Article, LinkedIn Post, Newsletter, YouTube Short, Slide Deck, Podcast/Audio
    - `Linked Media Project` — Relation (will be fully configured in Task 3.0)
    - `Date Decided` — Date
    - `Competing Content Found` — URL
  - [ ] 1.3 Verify all 12 properties are present with the correct types and option values
  - [ ] 1.4 Create a Board view as the default view, grouped by the `Status` property, with all status columns visible (including Abandoned)
  - [ ] 1.5 Create a second Table view showing all properties
  - [ ] 1.6 Add the database description: *"Every idea captured here. Volume at capture; judgment at the Decision Gate."*

- [ ] 2.0 Create Media Pipeline Database (N2)
  - [ ] 2.1 Create the "Media Pipeline" database as a direct child of the root page with the following properties and types:
    - `Project Title` — Title (default title field)
    - `Status` — Select with options in order: Scripting, Pre-Production, Recording, Editing, Packaging, Ready to Publish, Published
    - `Target Channel` — Select with options: YouTube, LinkedIn Video, Podcast, Webinar, Other
    - `Format Branch` — Select with options: A — Talking Head + Screen, B — Green Screen, C — Slide Voiceover
    - `Target Length` — Number (format: Number, represents minutes)
    - `Content Brief` — URL
    - `Performance Script` — URL
    - `Camtasia Project File` — Rich Text
    - `Export File` — Rich Text
    - `Publish Date` — Date
    - `Published URL` — URL
    - `7-Day CTR` — Number (format: Percent)
    - `7-Day Avg Duration` — Number (format: Number, represents seconds)
    - `30-Day Views` — Number
    - `30-Day Subscribers` — Number
    - `Notes` — Rich Text
    - `Linked Idea` — Relation (will be fully configured in Task 3.0)
  - [ ] 2.2 Verify all 17 properties are present with the correct types, formats, and option values
  - [ ] 2.3 Create a Board view as the default view, grouped by the `Status` property
  - [ ] 2.4 Create a second Table view showing all properties
  - [ ] 2.5 Add the database description: *"One entry per video project, from first script through published performance data."*

- [ ] 3.0 Configure two-way relation between Idea Registry and Media Pipeline
  - [ ] 3.1 Update the `Linked Media Project` property in Idea Registry to be a Relation pointing to the Media Pipeline database, configured as a two-way (synced) relation
  - [ ] 3.2 Confirm that the corresponding back-property (`Linked Idea`) appears automatically in Media Pipeline (Notion creates this automatically for two-way relations)
  - [ ] 3.3 Create one test entry in each database, link them via the relation property, and verify the reverse link populates in the other database automatically
  - [ ] 3.4 Delete the test entries after confirming the relation works correctly

- [ ] 4.0 Create Prompt Templates Vault page (N3)
  - [ ] 4.1 Create the "Prompt Templates Vault" page as a direct child of the root page
  - [ ] 4.2 Add the opening introduction paragraph explaining: what this vault is, how to use it (find your stage → expand the toggle → copy the prompt → fill in all `[INSERT: description]` fields before running), and a note that the canonical source is the GitHub prompt library (`prompts/` directory)
  - [ ] 4.3 Add **Stage 0 — Idea Governance & Registry** section:
    - Heading 2: "Stage 0 — Idea Governance & Registry"
    - One-sentence description of when to use these prompts
    - Toggle block: "SEO Landscape Scan Prompt" — body contains the full Perplexity prompt from `docs/media_production_system_20260221-01.md` (Stage 0.2 Step A), with all variables in `[INSERT: description]` format
    - Toggle block: "Differentiation Analysis Prompt" — body contains the full LLM prompt from Stage 0.2 Step C, with all variables in `[INSERT: description]` format
    - Toggle block: "Performance Notes" — empty body
  - [ ] 4.4 Add **Stage 1 — Ideation & Content Strategy** section:
    - Heading 2: "Stage 1 — Ideation & Content Strategy"
    - One-sentence description of when to use these prompts
    - Toggle block: "Title Finalization Prompt" — full prompt from Stage 1.1
    - Toggle block: "Hook Development Prompt" — full prompt from Stage 1.2
    - Toggle block: "Content Outline Prompt" — full prompt from Stage 1.3
    - Toggle block: "Performance Notes" — empty body
  - [ ] 4.5 Add **Stage 2 — Scripting & Content Structure** section:
    - Heading 2: "Stage 2 — Scripting & Content Structure"
    - One-sentence description of when to use these prompts
    - Toggle block: "Performance Script Prompt" — full prompt from Stage 2.1
    - Toggle block: "Shorts Extraction Prompt" — full prompt from Stage 2.3
    - Toggle block: "Performance Notes" — empty body
  - [ ] 4.6 Add **Stage 8 — Metadata & Packaging** section:
    - Heading 2: "Stage 8 — Metadata & Packaging"
    - One-sentence description of when to use these prompts
    - Toggle block: "Full Metadata Package Prompt (YouTube)" — full prompt from Stage 8.2
    - Toggle block: "Performance Notes" — empty body
  - [ ] 4.7 Add **Stage 10 — Analytics & Repurposing** section:
    - Heading 2: "Stage 10 — Analytics & Repurposing"
    - One-sentence description of when to use these prompts
    - Toggle block: "Repurposing Package Prompt" — full prompt from Stage 10.2
    - Toggle block: "Performance Notes" — empty body
  - [ ] 4.8 Review the completed page and verify: all prompt text is verbatim from the source document, all customization variables use `[INSERT: description]` format (no other variable formats), and every stage section ends with a "Performance Notes" toggle

- [ ] 5.0 Create Performance Insights Log page (N4)
  - [ ] 5.1 Create the "Performance Insights Log" page as a direct child of the root page
  - [ ] 5.2 Add the opening instruction text: *"Update this log after reviewing 7-day and 30-day analytics for each published video. This is your institutional memory for what works."*
  - [ ] 5.3 Add **Hooks That Worked** section:
    - Heading 2: "Hooks That Worked"
    - Callout block explaining what to log here (e.g., opening lines and title patterns from videos with high CTR or strong 30-second retention)
    - Table with columns: Hook Text | Video Title | CTR % | Notes — with one placeholder row of dashes
  - [ ] 5.4 Add **Thumbnail Patterns That Worked** section:
    - Heading 2: "Thumbnail Patterns That Worked"
    - Callout block explaining what to log here (e.g., visual style, text placement, face/no-face, color patterns from high-CTR thumbnails)
    - Table with columns: Visual Style Description | CTR % | Notes — with one placeholder row of dashes
  - [ ] 5.5 Add **Topics That Performed** section:
    - Heading 2: "Topics That Performed"
    - Callout block explaining what to log here (e.g., subject matter, audience angle, and why it resonated — based on 30-day view data)
    - Table with columns: Topic / Angle | Views at 30 Days | Notes — with one placeholder row of dashes
  - [ ] 5.6 Add **Topics That Underperformed** section:
    - Heading 2: "Topics That Underperformed"
    - Callout block explaining what to log here (e.g., topics that did not reach expectations and the likely reason — audience mismatch, poor hook, over-saturated topic)
    - Table with columns: Topic / Angle | Likely Reason | Notes — with one placeholder row of dashes
  - [ ] 5.7 Add **Prompt Refinements** section:
    - Heading 2: "Prompt Refinements"
    - Callout block explaining what to log here (e.g., changes made to prompts in the Vault, what the change was, and what improvement resulted)
    - Table with columns: Prompt Name | What Changed | Date | Result — with one placeholder row of dashes

- [ ] 6.0 Update root page with navigation block
  - [ ] 6.1 Read the current content of the root page (ID: `30f5b56017ff80a8a952d887c6fe9a7a`) to understand what already exists — do not overwrite or remove any existing content
  - [ ] 6.2 Add a navigation block at the very top of the root page (before any existing content) that links to all four structures:
    - Idea Registry (database link)
    - Media Pipeline (database link)
    - Prompt Templates Vault (page link)
    - Performance Insights Log (page link)
  - [ ] 6.3 Add a line in the navigation block for the GitHub repository with the placeholder text: `GitHub Repository — add URL after setup`

- [ ] 7.0 Create audit log file
  - [ ] 7.1 Check whether the `scripts/` directory exists in the repository root; create it if it does not
  - [ ] 7.2 Create `scripts/notion-build-log.md` with the following content:
    - Date of build
    - Name and Notion page/database ID of each of the four structures created
    - Name and ID of the root parent page
    - Note confirming the two-way relation between Idea Registry and Media Pipeline is configured
    - Note confirming the `Research Requested` status option exists in Idea Registry (exact string for Make trigger)
