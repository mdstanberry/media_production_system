# Stage 1 — Ideation & Content Strategy

**Description:** Transform a validated idea into a complete Content Brief — the single source of truth for the video.

## Prompts in This File

1. Title Finalization (LLM)
2. Hook Development (LLM)
3. Content Outline (LLM)

## When to Open This File

Open when you have a `Validated` idea in your Notion Idea Registry and are ready to build the Content Brief. Run all three prompts in sequence — each feeds into the next.

---

## Prompt 1 — Title Finalization

**Tool:** Claude or ChatGPT
**Stage step:** 1.1

### Before You Run This Prompt

- [ ] Idea status is `Validated` in Notion Idea Registry
- [ ] 3 candidate title options from Stage 0.3 (Differentiation Analysis) are ready to paste
- [ ] Target video length is decided

---

```
LLM PROMPT — TITLE FINALIZATION
---------------------------------
Here are 3 candidate titles for my YouTube video:
[PASTE TITLES]

My audience: FM professionals (want operational depth) and CRE executives (want strategic business implications)
Target length: [INSERT: target length, e.g., under 12 minutes]
My differentiation angle: [INSERT: differentiation angle from Stage 0]

For each title, assess:
- Clarity of outcome for the viewer
- Search discoverability
- Click-worthiness without being clickbait
- Audience alignment (FM practitioner vs. CRE executive — or both?)

Then recommend one final title and one alternate.
```

---

## Prompt 2 — Hook Development

**Tool:** Claude or ChatGPT
**Stage step:** 1.2

### Before You Run This Prompt

- [ ] Final title is selected from Prompt 1 output
- [ ] Core problem or question the video answers is clearly defined

---

```
LLM PROMPT — HOOK DEVELOPMENT
-------------------------------
Final title: [INSERT: final title]
Core audience: FM professionals and CRE executives
Key problem or question this video answers: [INSERT: core problem or question this video answers]

Write 3 alternative hooks for this video. Each hook should:
- Open with a pain point, provocative question, or surprising statement
- Establish credibility without being boastful
- Promise a specific, tangible outcome
- Be speakable and conversational (not academic)
- Run approximately 60–90 words (30–45 seconds on camera)

Label each: Hook A (problem-led), Hook B (question-led), Hook C (insight-led)
```

---

## Prompt 3 — Content Outline

**Tool:** Claude or ChatGPT
**Stage step:** 1.3

### Before You Run This Prompt

- [ ] Final title is confirmed
- [ ] Selected hook is chosen from Prompt 2 output
- [ ] Target video length is confirmed

---

```
LLM PROMPT — CONTENT OUTLINE
------------------------------
Final title: [INSERT: final title]
Chosen hook: [INSERT: paste selected hook from Prompt 2]
Target length: [INSERT: target length in minutes]
Audience: FM professionals and CRE executives

Create a structured content outline with:
- Intro segment (hook + context setup): target [INSERT: duration] minutes
- 3–5 content sections with section titles and key talking points per section
- CTA segment (what you want viewers to do): target 60–90 seconds
- Estimated duration per segment
- Visual intent per segment: A-roll only / screen demo / green screen / slide / B-roll overlay
- 2–3 key examples or analogies to use

Format as a run-of-show document.
```

---

## Performance Notes

