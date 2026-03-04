# Stage 10 — Analytics & Repurposing

**Description:** Log performance metrics and generate all content derivatives from the published video transcript.

## Prompts in This File

1. Repurposing Package (LLM)

## When to Open This File

Open after the video is published. The repurposing prompt is triggered by the `Repurposing Potential` tags set in Stage 0.4 — only generate the sections that match the tags on this video. Log performance metrics at 7 days and 30 days in the Notion Video Pipeline before running repurposing.

---

## Prompt 1 — Repurposing Package

**Tool:** Claude or ChatGPT
**Stage step:** 10.2

### Before You Run This Prompt

- [ ] Video is published and the URL is logged in Notion Video Pipeline
- [ ] Transcript is ready to paste
- [ ] Repurposing Potential tags from Stage 0.4 are confirmed — only request sections that apply
- [ ] 7-day performance metrics have been logged in Notion (or note if running this earlier)

---

```
LLM PROMPT — REPURPOSING PACKAGE
----------------------------------
Here is the transcript of my YouTube video: [INSERT: paste full transcript here]
Video title: [INSERT: video title]
Target audience: FM professionals and CRE executives

Generate the following based on applicable tags:

BLOG / ARTICLE:
- 800–1200 word article draft, 3rd person, authoritative tone
- Use the video content as the source — do not add outside information
- Include section headers matching the video structure

LINKEDIN POST:
- 150–250 word post, first person, professional but direct
- Open with the core insight, not "I just published a video"
- End with a question to drive comments
- Include video link placeholder

NEWSLETTER SNIPPET:
- 100–150 word summary for an email newsletter
- Focus on the "so what" for FM/CRE readers
- Include a link to the full video

YOUTUBE SHORTS (if tagged):
- 2–3 standalone Short scripts (30–60 seconds each)
- Each must open with a hook, deliver one key insight, end with a prompt
- Note which segment of the original video each Short derives from
```

---

## Performance Notes

