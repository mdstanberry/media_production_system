# PRD: Media Production System — Layer 3: Automation Specification

**Project:** Media Production System
**Layer:** 3 of 3
**Author:** Dean
**Date:** 2026-02-21
**Status:** Ready for Development
**Filename:** `prd-layer-3-automation-spec.md`
**Companion PRDs:** `prd-layer-1-documentation-config.md`, `prd-layer-2-notion-infrastructure.md`

---

## 1. Introduction / Overview

This PRD covers the third buildable layer of the Media Production System: the Make scenario specification document that defines the automated Perplexity API integration for SEO research. Layer 3 produces a single design and documentation artifact (`docs/make-scenario-spec.md`) — the Make scenario itself is configured manually in the Make UI using this spec as the authoritative blueprint.

**The problem it solves:** Stage 0.2 (SEO & Competitive Research, Step A) currently requires manually writing and submitting a Perplexity prompt for every idea under evaluation. The Make automation eliminates this manual step: when Dean changes an idea's status in Notion from `Raw` to `Research Requested`, Make fires the Perplexity Sonar API call automatically and writes the research output back to the Notion Idea Registry entry — requiring zero additional action from Dean.

**Goal:** Produce a self-contained `docs/make-scenario-spec.md` that is complete, unambiguous, and buildable by a third party with no prior context.

---

## 2. Goals

1. A self-contained Make scenario spec exists at `docs/make-scenario-spec.md` in the repository.
2. A third party who has never spoken to Dean can build the Make scenario in the Make UI from this document alone — no clarifying questions needed.
3. The spec explicitly documents the automated path (Make + Perplexity API) and a step-by-step manual fallback procedure.
4. All field mappings between Notion, Make, and the Perplexity API are unambiguously defined in a structured table.
5. The spec references the Layer 1 prompt library correctly, maintaining the system as the single source of truth for prompt content.

---

## 3. User Stories

- As Dean, I want to change an idea's Notion status to "Research Requested" and have the SEO research run automatically and appear in the Research Notes field — no manual prompt work required.
- As Dean (before Make is configured, or when Make is unavailable), I want to follow the manual fallback procedure and produce the same output as the automated scenario would.
- As a Make configurator (third party or future Dean), I want to open the scenario spec and have a complete, unambiguous build guide — every module type, field mapping, and configuration decision explicitly stated.
- As Dean, I want to know immediately if the automation fails — the Notion entry must reflect the error, not silently stay blank.

---

## 4. Functional Requirements

### 4.1 — Document Structure and Overview

1. The spec must open with a **plain-English summary** (3–5 sentences) describing: what the scenario does, when it runs, what the inputs are, and what the output looks like in Notion.

2. The spec must include a **text-based flow diagram** immediately after the summary, showing the complete trigger-to-output sequence. Minimum format:

   ```
   Notion: Status → "Research Requested"
           ↓
   Make: Read Idea Title + Target Channel from Notion entry
           ↓
   Make: Construct Perplexity prompt using template
           ↓
   Make: POST to Perplexity Sonar API
           ↓
   Make: Parse response → extract research text
           ↓
   Notion: Write research text → SEO Research Notes field
   Notion: Update Status → "Research Complete"
   ```

3. The spec must include a **table of contents** linking to each major section.

### 4.2 — Trigger Configuration

4. The spec must specify the Make trigger module: **Notion — Watch Database Items** (polls for new or updated items in a specified database).

5. Trigger condition: Notion Idea Registry item `Status` property changes to `Research Requested`.

6. The spec must state which Notion fields Make reads at trigger time (minimum): `Idea Title`, `Target Channel`, and the Notion item `ID` (required for the write-back step).

7. The spec must specify the recommended trigger polling interval: **every 15 minutes** (balances responsiveness with Make operation usage on the Free plan).

8. The spec must note that the Idea Registry **Notion Database ID** must be configured in the trigger module. It must document where to find this ID: open the database in Notion, click "..." → "Copy link to view," extract the 32-character ID from the URL.

### 4.3 — Prompt Construction

9. The spec must include the **exact Perplexity prompt template** used inside the Make HTTP module, with all dynamic fields replaced by Make variable references. Example format:

   ```
   Topic: {{1.properties.Idea Title.title[].plain_text}}
   Audience: Facilities Management professionals and Commercial Real Estate executives

   Please provide:
   1. Estimated search demand signals for this topic (high/medium/low and why)
   2. Top 3–5 related search queries people use to find this type of content
   3. Key sub-topics or angles that appear frequently in search results
   4. Any recent developments (past 6–12 months) that make this topic timely
   5. Assessment: Is this topic oversaturated, underdeveloped, or well-matched for a subject matter expert?
   ```

10. The prompt content must match the SEO Landscape Scan prompt from Stage 0.2 of the system document verbatim, with only the `[INSERT: description]` fields replaced by Make variable references. The spec must note that the canonical prompt source is `prompts/stage-00-idea-governance.md` in the repository.

11. The spec must state which Make module type handles the API call: **HTTP — Make a Request** (not a dedicated Perplexity module — none exists in Make as of the spec date).

### 4.4 — Perplexity API Configuration

12. The spec must document the Perplexity API endpoint: `https://api.perplexity.ai/chat/completions`

13. The spec must specify the model: `sonar` (Perplexity Sonar — cost-efficient; `sonar-pro` is noted as an upgrade option for higher-quality research output).

14. The spec must document the required HTTP request headers:
    - `Authorization: Bearer [INSERT: PERPLEXITY_API_KEY]`
    - `Content-Type: application/json`

15. The spec must include the **complete JSON request body structure** with Make variable placeholders:
    ```json
    {
      "model": "sonar",
      "messages": [
        {
          "role": "user",
          "content": "[INSERT: constructed prompt text with Make variables]"
        }
      ],
      "max_tokens": 1024
    }
    ```

16. The spec must specify where the API key is stored: **Make connection credential** (configured once in Make → Connections, not hardcoded in the scenario). The key name in `.env.example` is `PERPLEXITY_API_KEY`.

17. The spec must note that `max_tokens: 1024` is the recommended starting value and may be increased if research responses are being truncated.

### 4.5 — Response Handling

18. The spec must document how Make extracts the research output from the Perplexity API response, including the **exact JSON path**: `choices[0].message.content`.

19. The spec must specify Make's module reference syntax for this value (for use in the write-back step): `{{2.choices[].message.content}}` (or equivalent based on Make's array notation).

20. The spec must define error handling behavior:
    - **If Perplexity returns an HTTP error (4xx/5xx):** Make must write `[SEO Research Error — automated scan failed. Run the manual fallback procedure in docs/make-scenario-spec.md]` to the `SEO Research Notes` field in Notion, update the Status to `Research Complete` (so the item doesn't re-trigger), and send an error notification via Make's built-in error handler.
    - **If the response body is empty or malformed:** Same write-back behavior as above.
    - **Design principle:** The scenario must never fail silently. Dean must always know whether research ran successfully or needs manual follow-up.

### 4.6 — Notion Write-Back

21. The spec must specify the Make module for the write-back: **Notion — Update a Database Item**.

22. The spec must document the exact field mappings for the write-back step:

    | Notion Field | Value Written | Source |
    |---|---|---|
    | SEO Research Notes | Perplexity response content | `{{2.choices[].message.content}}` |
    | Status | `Research Complete` | Static value |

23. The spec must note that the Notion item ID (from the trigger module) must be passed to the write-back module to identify which entry to update.

### 4.7 — Complete Field Mapping Table

24. The spec must include a **complete field mapping table** covering the full data flow from Notion trigger through Notion write-back:

    | Step | Source | Field / Value | Make Variable | Destination |
    |---|---|---|---|---|
    | 1 — Trigger | Notion Idea Registry | Idea Title | `{{1.properties.Idea Title.title[].plain_text}}` | Perplexity prompt: Topic field |
    | 2 — Trigger | Notion Idea Registry | Target Channel | `{{1.properties.Target Channel.select.name}}` | Available for prompt personalization (optional) |
    | 3 — Trigger | Notion Idea Registry | Item ID | `{{1.id}}` | Notion write-back: item identifier |
    | 4 — API Response | Perplexity API | `choices[0].message.content` | `{{2.choices[].message.content}}` | Notion: SEO Research Notes |
    | 5 — Write-Back | Static value | `Research Complete` | — | Notion: Status |

### 4.8 — Manual Fallback Procedure

25. The spec must include a numbered, step-by-step **manual fallback procedure** for running the same research process when Make is not configured or unavailable. The procedure must be self-contained — no other document required to follow it.

26. The fallback must reference the Stage 0 prompt template file: `prompts/stage-00-idea-governance.md` (from the Layer 1 repository) for the exact prompt text.

27. The fallback procedure must specify exactly where to paste the Perplexity output in Notion: the `SEO Research Notes` field in the Idea Registry database, then manually update Status to `Research Complete`.

28. Minimum fallback procedure format:
    1. Open Perplexity (perplexity.ai)
    2. Open `prompts/stage-00-idea-governance.md` in the repository
    3. Copy the "SEO Landscape Scan Prompt"
    4. Fill in all `[INSERT: description]` fields with your idea's working title and audience details
    5. Paste and submit the completed prompt in Perplexity
    6. Copy the full response
    7. Open the Idea Registry in Notion; find the idea entry
    8. Paste the response into the `SEO Research Notes` field
    9. Update `Status` → `Research Complete`

### 4.9 — Setup Requirements Checklist

29. The spec must end with a **setup checklist** that a Make configurator must complete before the scenario can run:

    - [ ] Perplexity API key obtained from perplexity.ai/settings/api
    - [ ] Perplexity API key stored as a Make connection credential (not hardcoded)
    - [ ] Notion connection authorized in Make (OAuth or integration token)
    - [ ] Notion Idea Registry database ID confirmed and entered in the trigger module
    - [ ] Scenario tested with one real Idea Registry entry (manually set Status to "Research Requested" and verify write-back)
    - [ ] Error handling verified: test with an intentionally invalid API key and confirm the error message appears in Notion

---

## 5. Non-Goals (Out of Scope for This Layer)

- **Configuring the Make scenario in the Make UI.** The spec document only. Make UI configuration is manual work done post-build using this spec as the guide.
- **Automating any production stage other than Stage 0.2 Step A** (SEO Landscape Scan). All other stages remain manual or Cowork-assisted (Phase 2).
- **Perplexity API rate limiting or retry logic.** Noted as a future enhancement; not required in v1. The error handler covers failure cases.
- **Any Make scenario for channels other than YouTube.** The architecture supports extension; channel-specific prompt variations are not in scope for this spec.
- **Make scenario configuration for Stage 8, Stage 10, or any other automated workflow.** v1 automates one step only.

---

## 6. Technical Considerations

- **Perplexity API:** No dedicated Make module exists. Use **HTTP — Make a Request** with `POST` method. API key stored as a Make connection credential.
- **Make plan compatibility:** The scenario uses 2 operations per run (trigger read + Notion write). Compatible with Make Free tier. At high volume (many ideas evaluated simultaneously), monitor operation count.
- **Notion API:** Make's native Notion module handles authentication. The Idea Registry database ID must be confirmed at Make setup time (after Layer 2 build creates the database).
- **Make variable notation:** Make uses `{{moduleNumber.propertyPath}}` notation. The exact variable names depend on how Make labels the trigger module (default: module 1) and HTTP module (default: module 2). The spec should note this and instruct the configurator to verify variable paths using Make's variable picker.
- **`sonar` vs `sonar-pro`:** Use `sonar` by default. Note in the spec that `sonar-pro` delivers higher-quality research but costs more per call. Upgrade is a one-field change in the request body.
- **Prompt stored in Make:** The prompt template is stored as a Make text variable within the HTTP module's request body — not fetched from GitHub at runtime. If the prompt is updated in `prompts/stage-00-idea-governance.md`, the Make scenario must be manually updated to match.

---

## 7. Success Metrics

1. `docs/make-scenario-spec.md` is complete and present in the repository.
2. Dean reviews the document and confirms: *"A third party could build this scenario from this document alone — no questions needed."*
3. The manual fallback procedure is tested by Dean at least once before Make is configured: Dean follows the steps and confirms the output appears correctly in Notion.
4. When the Make scenario is configured using this spec, it runs end-to-end without modification to the spec document.
5. An error condition is tested (invalid API key) and Dean confirms the error message appears in the Notion `SEO Research Notes` field as specified.

---

## 8. Open Questions

- **Should the spec include screenshots of the Make UI for each module?** Recommendation: no — Make UI changes frequently and screenshots become outdated within months. Use module names, setting labels, and field descriptions only. The spec remains accurate longer without screenshots.
- **Perplexity model version pinning:** Should the spec pin to a specific model version (e.g., `sonar-2025`) or leave as `sonar` (always latest)? Recommendation: `sonar` (latest) — Perplexity's Sonar models improve over time. If a specific version is needed for consistency, note it as a comment in the spec, not a hard requirement.
- **Make scenario naming convention:** The spec should specify what to name the scenario in Make for findability. Recommendation: `MPS — Idea Research Automation` (MPS = Media Production System).

---

*Media Production System — Layer 3 PRD v1.0*
*Input: `media_production_system_feature_brief_20260221-01.md` + clarifying answers | Dean | 2026-02-21*
*Dependency: Layer 2 must be built first — the Idea Registry database ID is required for Make trigger configuration*
