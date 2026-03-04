# Stage 0 — Idea Governance & Registry

**Description:** Evaluate raw ideas using SEO research and competitive analysis before committing time to production.

## Prompts in This File

1. SEO Landscape Scan (Perplexity)
2. Differentiation Analysis (LLM — Claude or ChatGPT)

## When to Open This File

Open when you have a `Raw` idea in your Notion Idea Registry and are ready to run the Stage 0.2 research workflow. This file covers Steps A and C of that workflow. Step B (YouTube manual scan) is done directly in YouTube — no prompt needed.

---

## Prompt 1 — SEO Landscape Scan

**Tool:** Perplexity
**Stage step:** 0.2 — Step A

### Before You Run This Prompt

- [ ] Working title or topic phrase is captured in Notion Idea Registry
- [ ] Idea status is `Raw`
- [ ] You are in Perplexity (this prompt is designed for Perplexity, not Claude or ChatGPT)

---

```
PERPLEXITY PROMPT — SEO LANDSCAPE SCAN
---------------------------------------
Topic: [INSERT WORKING TITLE OR TOPIC]
Audience: Facilities Management professionals and Commercial Real Estate executives

Please provide:
1. Estimated search demand signals for this topic (high/medium/low and why)
2. Top 3–5 related search queries people use to find this type of content
3. Key sub-topics or angles that appear frequently in search results
4. Any recent developments (past 6–12 months) that make this topic timely
5. Assessment: Is this topic oversaturated, underdeveloped, or well-matched for a subject matter expert?
```

---

## Prompt 2 — Differentiation Analysis

**Tool:** Claude or ChatGPT
**Stage step:** 0.2 — Step C

### Before You Run This Prompt

- [ ] Perplexity SEO Landscape Scan (Prompt 1 above) is complete
- [ ] YouTube manual competitive scan (Step B) is complete — notes ready to paste
- [ ] Your unique angle or perspective is articulated

---

```
LLM PROMPT — DIFFERENTIATION ANALYSIS
---------------------------------------
Working title: [INSERT: working title]
Target audience: FM professionals and CRE executives
What I found on YouTube: [INSERT: paste notes from YouTube competitive scan]
My unique angle / perspective: [INSERT: your unique angle or perspective, e.g., "25 years in FM operations"]

Please assess:
1. Is this topic already well-covered for my specific audience?
2. What differentiation angle would make my video worth watching over existing results?
3. Suggest 3 refined title options that signal clear differentiation
4. Recommend: Accept this topic, Refine the angle, or Abandon
5. If Abandon — suggest a related angle worth exploring instead
```

---

## Performance Notes

