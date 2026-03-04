## Relevant Files

- `docs/make-scenario-spec.md` - The primary deliverable for this layer. A self-contained Make scenario build guide.
- `prompts/stage-00-idea-governance.md` - Source of truth for the SEO Landscape Scan prompt template referenced in the spec.
- `prd-layer-3-automation-spec.md` - The PRD this task list is based on; refer back to it if any section requirements are unclear.
- `tasks/tasks-layer-2-notion-infrastructure.md` - Layer 2 tasks; the Notion Idea Registry database ID produced there is needed for the Make trigger configuration.

### Notes

- This layer produces a single documentation artifact: `docs/make-scenario-spec.md`.
- No code is written in this layer. All tasks involve authoring, structuring, and verifying a spec document.
- The Make scenario itself is configured manually in the Make UI _after_ this spec is complete — the spec is the build guide.
- Dependency: Layer 2 must be complete before the Make trigger can be tested, because the Notion Idea Registry database ID is required.

## Instructions for Completing Tasks

**IMPORTANT:** As you complete each task, check it off by changing `- [ ]` to `- [x]`. Update after each sub-task, not just after the parent task.

Example:
- `- [ ] 1.1 Read file` → `- [x] 1.1 Read file` (after completing)

## Tasks

- [ ] 0.0 Create feature branch
  - [ ] 0.1 Create and checkout a new branch: `git checkout -b feature/layer-3-automation-spec`

- [ ] 1.0 Create `docs/make-scenario-spec.md` with document structure
  - [ ] 1.1 Create the `docs/` folder at the repository root if it does not already exist
  - [ ] 1.2 Create `docs/make-scenario-spec.md` with a document header (title, date, status, companion documents)
  - [ ] 1.3 Write the plain-English summary section (3–5 sentences covering: what the scenario does, when it runs, what the inputs are, and what the output looks like in Notion)
  - [ ] 1.4 Add the text-based flow diagram showing the full trigger-to-output sequence (Notion status change → Make reads fields → prompt constructed → Perplexity API call → response parsed → Notion write-back)
  - [ ] 1.5 Add a table of contents with anchor links to every major section in the document

- [ ] 2.0 Write trigger configuration section
  - [ ] 2.1 Specify the Make trigger module: **Notion — Watch Database Items**
  - [ ] 2.2 Document the trigger condition: Notion Idea Registry `Status` property changes to `Research Requested`
  - [ ] 2.3 List the Notion fields Make reads at trigger time: `Idea Title`, `Target Channel`, and the Notion item `ID`
  - [ ] 2.4 Specify the recommended polling interval: every 15 minutes, and explain the reason (balances responsiveness with Make Free plan operation usage)
  - [ ] 2.5 Document how to find and configure the Notion Idea Registry database ID (open database in Notion → "..." → "Copy link to view" → extract the 32-character ID from the URL)

- [ ] 3.0 Write prompt construction and Perplexity API configuration section
  - [ ] 3.1 Include the exact Perplexity prompt template with all dynamic fields replaced by Make variable references (e.g., `{{1.properties.Idea Title.title[].plain_text}}`)
  - [ ] 3.2 Note that the canonical prompt source is `prompts/stage-00-idea-governance.md` and that if the prompt is updated there, the Make scenario must be manually updated to match
  - [ ] 3.3 Specify the Make module type for the API call: **HTTP — Make a Request** (note: no dedicated Perplexity module exists in Make as of the spec date)
  - [ ] 3.4 Document the Perplexity API endpoint: `https://api.perplexity.ai/chat/completions` with HTTP method `POST`
  - [ ] 3.5 Specify the model: `sonar` as default; note `sonar-pro` as an upgrade option for higher-quality output (one-field change)
  - [ ] 3.6 Document the required HTTP request headers: `Authorization: Bearer [PERPLEXITY_API_KEY]` and `Content-Type: application/json`
  - [ ] 3.7 Include the complete JSON request body structure with Make variable placeholders (`model`, `messages` array with `role` and `content`, `max_tokens: 1024`)
  - [ ] 3.8 Document where the API key is stored: as a Make connection credential (configured once in Make → Connections, not hardcoded); key name in `.env.example` is `PERPLEXITY_API_KEY`
  - [ ] 3.9 Note that `max_tokens: 1024` is the recommended starting value and may be increased if responses are being truncated

- [ ] 4.0 Write response handling and error behavior section
  - [ ] 4.1 Document how Make extracts the research output from the Perplexity API response, including the exact JSON path: `choices[0].message.content`
  - [ ] 4.2 Specify Make's module reference syntax for the extracted value: `{{2.choices[].message.content}}`
  - [ ] 4.3 Add a note that exact variable paths depend on Make's module numbering and should be verified using Make's variable picker when building the scenario
  - [ ] 4.4 Define error handling behavior for HTTP errors (4xx/5xx): write a specific error message to `SEO Research Notes`, update Status to `Research Complete`, and send an error notification via Make's built-in error handler
  - [ ] 4.5 Define error handling behavior for empty or malformed API responses: same write-back behavior as HTTP errors
  - [ ] 4.6 State the design principle explicitly: the scenario must never fail silently — Dean must always know whether research ran successfully or needs manual follow-up

- [ ] 5.0 Write Notion write-back and complete field mapping table section
  - [ ] 5.1 Specify the Make module for the write-back step: **Notion — Update a Database Item**
  - [ ] 5.2 Document the write-back field mappings in a table: `SEO Research Notes` ← Perplexity response (`{{2.choices[].message.content}}`); `Status` ← `Research Complete` (static value)
  - [ ] 5.3 Note that the Notion item ID from the trigger module must be passed to the write-back module to identify which entry to update
  - [ ] 5.4 Include the complete field mapping table covering the full data flow across all 5 steps (trigger read → prompt construction → API call → response extraction → Notion write-back), as specified in PRD section 4.7

- [ ] 6.0 Write manual fallback procedure section
  - [ ] 6.1 Write an introduction explaining when to use the fallback (Make not configured, Make unavailable, or testing before automation is live)
  - [ ] 6.2 Write the numbered step-by-step fallback procedure (9 steps as specified in PRD section 4.8): open Perplexity → open prompt file → copy SEO Landscape Scan prompt → fill in fields → paste and submit → copy response → open Notion Idea Registry → paste into `SEO Research Notes` → update Status to `Research Complete`
  - [ ] 6.3 Confirm the procedure is fully self-contained — a reader should be able to follow it with no other document open (except the referenced prompt file)

- [ ] 7.0 Add setup requirements checklist and finalize document
  - [ ] 7.1 Add the setup requirements checklist at the end of the document (6 items as specified in PRD section 4.9): API key obtained, API key stored as Make credential, Notion connection authorized, database ID confirmed, end-to-end test completed, error handling verified
  - [ ] 7.2 Add the Make scenario naming recommendation: `MPS — Idea Research Automation`
  - [ ] 7.3 Review the complete document against all 29 functional requirements in PRD sections 4.1–4.9 and confirm each is addressed; check off any gaps
  - [ ] 7.4 Verify all cross-references are accurate: `prompts/stage-00-idea-governance.md` path, Notion field names match Layer 2 spec, Make module names are correct
  - [ ] 7.5 Commit the completed `docs/make-scenario-spec.md` and this task file, then open a pull request to merge the feature branch into `main`
