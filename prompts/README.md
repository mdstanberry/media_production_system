# prompts/

This directory is the prompt template library for the Media Production System. Each file contains the ready-to-use LLM prompts for one stage of the production workflow.

## Files in This Directory

| File | Stage | Prompts |
|------|-------|---------|
| `stage-00-idea-governance.md` | Stage 0 — Idea Governance | SEO Landscape Scan (Perplexity), Differentiation Analysis |
| `stage-01-content-strategy.md` | Stage 1 — Content Strategy | Title Finalization, Hook Development, Content Outline |
| `stage-02-scripting.md` | Stage 2 — Scripting | Performance Script, Shorts Extraction (optional) |
| `stage-03-pre-production.md` | Stage 3 — Pre-Production | No LLM prompts — operational checklist only |
| `stage-08-metadata-packaging.md` | Stage 8 — Metadata & Packaging | Full Metadata Package (YouTube) |
| `stage-10-analytics-repurposing.md` | Stage 10 — Analytics & Repurposing | Repurposing Package |

## Variable Convention

All customization placeholders in every prompt use this format:

```
[INSERT: description]
```

The description tells you exactly what to put there. Examples:
- `[INSERT: final title]` — type your confirmed video title
- `[INSERT: working title or topic]` — type the idea phrase from your Notion registry

**No other formats are used.** If you see `[PASTE]`, `{{variable}}`, `<variable>`, or a bare `[INSERT]` with no description, that is an error — correct it to `[INSERT: description]`.

## How to Use These Files in Cursor Agent

You can reference any prompt file directly in a Cursor Agent conversation using the `@` symbol:

```
@stage-08-metadata-packaging.md
```

This loads the file into the agent's context so it can see the prompt template and help you fill it in or run it.

**Recommended workflow:**
1. Open the stage file for your current production step
2. Complete the "Before You Run This Prompt" checklist
3. Copy the prompt block into your LLM tool (Claude, ChatGPT, or Perplexity)
4. Replace all `[INSERT: description]` placeholders with your actual content
5. After running, note what worked in the `## Performance Notes` section at the bottom of the file

## Source of Truth

All prompt content in this library was copied verbatim from `docs/media-production-system.md`. That file is the authoritative system reference. If a prompt ever needs updating, update `docs/media-production-system.md` first, then sync the change here.
