# Stage 8 — Metadata & Packaging

**Description:** Generate the complete SEO and metadata package for the video — the highest-leverage stage for discoverability.

## Prompts in This File

1. Full Metadata Package — YouTube (LLM)

## When to Open This File

Open after the video has been exported and a transcript is available. Do not rush this stage. Channel-specific requirements are defined in `channels/youtube.md`. This prompt is pre-configured for YouTube — adapt field labels for other channels when they become active.

---

## Prompt 1 — Full Metadata Package (YouTube)

**Tool:** Claude or ChatGPT
**Stage step:** 8.2

### Before You Run This Prompt

- [ ] Video is exported and file is confirmed at correct path and naming convention
- [ ] Transcript is ready to paste (generated via Claude upload, YouTube auto-captions, or Camtasia)
- [ ] Final title is confirmed
- [ ] `channels/youtube.md` has been reviewed for any platform-specific requirements

---

```
LLM PROMPT — FULL METADATA PACKAGE (YOUTUBE)
----------------------------------------------
Here is the transcript/script for my YouTube video: [INSERT: paste full transcript or script here]

Video title: [INSERT FINAL TITLE]
Target audience: FM professionals and CRE executives
Channel focus: Building Lifecycle Management, Facilities Management, PropTech, Smart Buildings, Decarbonization

Generate the following:

1. TITLE OPTIONS (5 variants)
   - Optimize for search + click-through rate
   - Keep under 60 characters
   - Label: [Search-led], [Curiosity-led], [Outcome-led], [Authority-led], [Urgency-led]

2. DESCRIPTION (full)
   - Opening paragraph: 2–3 sentences summarizing the video (appears above "Show more")
   - Body: Key topics covered, each as a short paragraph
   - CTA: Subscribe prompt and related video/resource mention
   - Chapter markers: [TIMECODE] — Section Title format (I will add real timecodes)
   - Hashtags: 3–5 relevant hashtags at the end

3. TAGS (15–20)
   - Mix of broad, mid-tail, and long-tail keywords
   - Include FM, CRE, PropTech, and topic-specific terms

4. CHAPTER LIST
   - Based on the content outline, formatted for YouTube chapters

5. CARDS & END SCREEN NOTES
   - Suggest 2–3 moments in the video where a card to a related topic would perform well
```

---

## Performance Notes

