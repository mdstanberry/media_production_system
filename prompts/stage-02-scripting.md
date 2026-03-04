# Stage 2 — Scripting & Content Structure

**Description:** Convert the Content Brief into a Performance Script — speaking bullets optimized for on-camera or voiceover delivery.

## Prompts in This File

1. Performance Script (LLM)
2. Shorts Extraction (LLM) — optional, only if video has a `YouTube Short` repurposing tag

## When to Open This File

Open when the Content Brief from Stage 1 is complete and saved in Notion. Run Prompt 1 first. Only run Prompt 2 if the video was tagged for `YouTube Short` repurposing in Stage 0.4.

---

## Prompt 1 — Performance Script

**Tool:** Claude or ChatGPT
**Stage step:** 2.1

### Before You Run This Prompt

- [ ] Content outline from Stage 1 (Prompt 3) is complete and ready to paste
- [ ] Content Brief is saved in Notion

---

```
LLM PROMPT — PERFORMANCE SCRIPT
---------------------------------
Here is my content outline: [PASTE OUTLINE]

Rewrite this as a performance script for on-camera delivery:
- Convert each section into speaking bullets: short, conversational, easy to scan while recording
- Each bullet = one complete thought (1–2 sentences max when spoken)
- Add ANCHOR PHRASES: key sentences I should say nearly verbatim (for quotability and clarity)
- Add TRANSITION LINES between sections
- Flag any technical terms that need plain-language alternatives for CRE executive viewers
- Include [PAUSE] markers where I should let a point land before continuing
- Include [DEMO] or [SLIDE] markers where screen content or visuals take over

Format with clear segment headers matching the outline.
```

---

## Prompt 2 — Shorts Extraction *(optional)*

**Tool:** Claude or ChatGPT
**Stage step:** 2.3

### Before You Run This Prompt

- [ ] Video has a `YouTube Short` repurposing tag (set in Stage 0.4)
- [ ] Performance script from Prompt 1 is complete and ready to paste

---

```
LLM PROMPT — SHORTS EXTRACTION
--------------------------------
Here is my performance script: [PASTE SCRIPT]

Identify 2–3 segments that could stand alone as a 30–60 second YouTube Short.
For each:
- Quote the segment (verbatim from script)
- Suggest a standalone hook line to open the Short
- Suggest a title for the Short
- Note if any visual adaptation is needed
```

---

## Performance Notes

