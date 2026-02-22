# Media Production System
### AI-Augmented Solo Creator Workflow
**Author:** Dean | **Version:** 1.0 | **Date:** 2026-02-21

---

> **System Design Philosophy:** This is a modular, re-enterable production system built for an irregular publishing cadence. Every stage produces a durable artifact stored in Notion. You should be able to walk away for six weeks and resume any media project without losing context or re-inventing decisions. YouTube is the initial supported channel; the architecture is channel-agnostic to support future expansion (LinkedIn Video, Podcast, Webinar, etc.) without requiring a rebuild.

---

## System Architecture Overview

```
SHARED STAGES (all formats, all channels)
│
├── Stage 0 │ Idea Governance & Registry
├── Stage 1 │ Ideation & Content Strategy
├── Stage 2 │ Scripting & Content Structure
├── Stage 3 │ Pre-Production Planning
│
FORMAT BRANCHES (diverge here)
│
├── Branch A │ Talking Head + Screen Demo
├── Branch B │ Talking Head / Green Screen
├── Branch C │ Slide-Driven Voiceover
│
SHARED STAGES (resume here)
│
├── Stage 7 │ Edit & Assembly (Camtasia)
├── Stage 8 │ SEO & Metadata Packaging
├── Stage 9 │ Publishing Checklist (channel-specific)
└── Stage 10 │ Analytics & Repurposing
```

**Channel Support:**
- **v1.0 — Active:** YouTube
- **v2.0 — Planned:** LinkedIn Video, Podcast, Webinar

Channel-specific elements (publishing checklists, SEO/metadata field labels, platform requirements) are isolated in their own configuration files. Adding a new channel requires adding a configuration — not modifying shared infrastructure.

**Tech Stack Roles:**
- **Notion** — Production memory, Idea Registry, pipeline tracker, prompt vault, performance log
- **Claude / ChatGPT / Gemini** — Scripting, content structure, SEO/metadata packaging, repurposing
- **Perplexity** — SEO landscape research, competitive scanning
- **Make** — Automation bridge (Notion → Perplexity API → Notion)
- **Camtasia** — Primary NLE: screen recording, A-roll editing, chroma key, templates
- **Canva** — Thumbnails, backgrounds, title cards, in-video graphics
- **Cowork** *(Phase 2)* — Execution layer for script assembly and metadata packaging from local files

**Channel Configuration Files** *(isolated, additive — do not modify shared stages to add a channel):*
- `channels/youtube.md` — YouTube-specific publishing checklist, SEO fields, metadata requirements
- `channels/linkedin-video.md` *(planned)*
- `channels/podcast.md` *(planned)*

---

## STAGE 0 — Idea Governance & Registry

> **Purpose:** Capture every idea before it disappears. Validate before investing time. Build an institutional memory of what you've accepted, refined, and abandoned — so you never duplicate effort or repeat failed experiments.

### 0.1 Idea Capture

**Trigger:** Any moment an idea surfaces — conversation, article, client question, industry event.

**Action:** Log immediately to Notion Idea Registry (see Notion Schema, Section N3). Minimum viable entry:
- Working title or topic phrase
- Source / trigger (what prompted this)
- Date captured
- Status: `Raw`

**Rule:** Do not filter at capture. Volume is the goal at this step. Judgment comes at the Decision Gate.

---

### 0.2 SEO & Competitive Research

**Trigger:** When you're ready to evaluate a `Raw` idea. This can be batched — evaluate 5–10 ideas in one sitting.

**Tools:** Perplexity (primary), manual YouTube search (required), Google search (secondary)

**Step-by-step:**

**Step A — Perplexity SEO Landscape Scan**

Use this prompt template in Perplexity:

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

**Step B — YouTube Manual Competitive Scan**

Search your working title verbatim and 2–3 variant phrasings. For the top 5–10 results, note:

| Field | What to capture |
|---|---|
| Video count | How many results exist? |
| View counts | Are they getting traction (10K+) or dormant? |
| Age of top results | Fresh (< 1 year) or stale (3+ years)? |
| Typical length | Under 10 min? 10–20? Long-form? |
| Thumbnail patterns | What visual style dominates? |
| Title patterns | Question-based? How-to? List? |
| Obvious gaps | What angle is missing? What's not being said? |

**Step C — Differentiation Analysis**

Run this LLM prompt (Claude or ChatGPT):

```
LLM PROMPT — DIFFERENTIATION ANALYSIS
---------------------------------------
Working title: [INSERT]
Target audience: FM professionals and CRE executives
What I found on YouTube: [PASTE YOUR NOTES FROM STEP B]
My unique angle / perspective: [INSERT — e.g., "25 years in FM operations"]

Please assess:
1. Is this topic already well-covered for my specific audience?
2. What differentiation angle would make my video worth watching over existing results?
3. Suggest 3 refined title options that signal clear differentiation
4. Recommend: Accept this topic, Refine the angle, or Abandon
5. If Abandon — suggest a related angle worth exploring instead
```

---

### 0.3 Decision Gate

Review research outputs and make a formal decision. Update the Notion Idea Registry entry:

| Decision | Status Tag | Next Action |
|---|---|---|
| **Accept** | `Validated` | Move to Stage 1 |
| **Refine** | `Needs Refinement` | Re-run research with adjusted angle |
| **Abandon** | `Abandoned` | Log reason; record in registry permanently |

> **Critical:** Abandoned ideas are never deleted. They are institutional memory. Every new idea intake should cross-reference the registry to avoid repeating failed explorations.

### 0.4 Repurposing Potential Flag

At Decision Gate, also flag repurposing potential. Tag each accepted idea with applicable content types:

- `Blog / Article`
- `LinkedIn Post`
- `Newsletter`
- `YouTube Short (30–60s)`
- `Slide Deck / Canva Presentation`
- `Podcast / Audio`

This tag drives Stage 10 (Repurposing) automatically — no decisions required later.

---

## STAGE 1 — Ideation & Content Strategy

> **Input:** A `Validated` idea from the Idea Registry.
> **Output:** Content Brief — the single source of truth for this video.

### 1.1 Title Refinement

Take the differentiated title options from Stage 0.3. Run this prompt to finalize:

```
LLM PROMPT — TITLE FINALIZATION
---------------------------------
Here are 3 candidate titles for my YouTube video:
[PASTE TITLES]

My audience: FM professionals (want operational depth) and CRE executives (want strategic business implications)
Target length: [INSERT — e.g., under 12 minutes]
My differentiation angle: [INSERT]

For each title, assess:
- Clarity of outcome for the viewer
- Search discoverability
- Click-worthiness without being clickbait
- Audience alignment (FM practitioner vs. CRE executive — or both?)

Then recommend one final title and one alternate.
```

### 1.2 Hook Development

The hook is the first 30–60 seconds of your video. It determines retention. Draft it before you write the full outline.

```
LLM PROMPT — HOOK DEVELOPMENT
-------------------------------
Final title: [INSERT]
Core audience: FM professionals and CRE executives
Key problem or question this video answers: [INSERT]

Write 3 alternative hooks for this video. Each hook should:
- Open with a pain point, provocative question, or surprising statement
- Establish credibility without being boastful
- Promise a specific, tangible outcome
- Be speakable and conversational (not academic)
- Run approximately 60–90 words (30–45 seconds on camera)

Label each: Hook A (problem-led), Hook B (question-led), Hook C (insight-led)
```

### 1.3 Content Outline

```
LLM PROMPT — CONTENT OUTLINE
------------------------------
Final title: [INSERT]
Chosen hook: [PASTE SELECTED HOOK]
Target length: [INSERT minutes]
Audience: FM professionals and CRE executives

Create a structured content outline with:
- Intro segment (hook + context setup): target [X] minutes
- 3–5 content sections with section titles and key talking points per section
- CTA segment (what you want viewers to do): target 60–90 seconds
- Estimated duration per segment
- Visual intent per segment: A-roll only / screen demo / green screen / slide / B-roll overlay
- 2–3 key examples or analogies to use

Format as a run-of-show document.
```

### 1.4 Content Brief Assembly

Compile the following into a single Notion page (linked to the Idea Registry entry):

- Final title + alternate title
- Selected hook (verbatim)
- Content outline / run-of-show
- Target length
- Format branch: A, B, or C
- Visual intent notes
- Repurposing potential tags (from Stage 0.4)
- Thumbnail concept (initial rough idea — developed in Stage 8)

---

## STAGE 2 — Scripting & Content Structure

> **Input:** Content Brief from Stage 1.
> **Output:** Performance Script — speaking bullets optimized for on-camera or voiceover delivery.

### 2.1 Script Development

This system uses **structured bullets plus anchor phrases** — not a word-for-word teleprompter script. The goal is natural delivery with consistent messaging.

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

### 2.2 Script Review & Personalization

**Manual step — do not skip.** Read the script aloud. Identify:
- Phrases that don't sound like you — rewrite them
- Technical terms that need examples or analogies
- Sections that run long — tighten to bullet, not prose
- Moments where your experience or specific stories add credibility — insert them

Estimate final read-through time. If over target length by more than 15%, cut a section — don't compress delivery speed.

### 2.3 Shorts Extraction (Optional)

If the video has a `YouTube Short` repurposing tag:

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

## STAGE 3 — Pre-Production Planning

> **Input:** Performance Script + Content Brief.
> **Output:** Production Package — everything you need before you press Record.

### 3.1 Asset Checklist

Review the Visual Intent notes from the Content Brief and prepare:

**For all formats:**
- [ ] Performance script printed or on second screen
- [ ] Notion Content Brief open and accessible
- [ ] Camtasia project file created from master template
- [ ] Recording environment checked (lighting, background, noise)
- [ ] Teleprompter app loaded (if using)

**Branch A (Talking Head + Screen Demo) additions:**
- [ ] Screen content prepared: browser tabs, apps, documents pre-loaded
- [ ] Screen recording region defined in Camtasia
- [ ] Demo walk-through rehearsed once

**Branch B (Talking Head / Green Screen) additions:**
- [ ] Green screen lit evenly — no hotspots or shadows
- [ ] Background asset(s) designed in Canva and exported (1920×1080 PNG or MP4)
- [ ] Camera framing checked with background composite in mind

**Branch C (Slide-Driven Voiceover) additions:**
- [ ] Slide deck completed in Canva or PowerPoint
- [ ] Slides exported or screen-recorded source confirmed
- [ ] Voiceover delivery plan: live recording vs. segment-by-segment

### 3.2 Canva Asset Preparation

**Backgrounds (Branch B):**
Create 1920×1080 background(s) in Canva. Guidelines:
- Low visual complexity — you are the focus, not the background
- On-brand colors and subtle motion acceptable (Canva video backgrounds)
- Create 2–3 background variants per brand style for variety across videos

**Title Cards / Section Panels:**
- Create reusable section card template in Canva (branded, text-swappable)
- Export as PNG for Camtasia overlay

**Thumbnail (Draft):**
- Pull a representative frame concept from the Content Brief
- Note: Final thumbnail produced in Stage 8 after recording

### 3.3 Camtasia Template Setup

Maintain one master Camtasia project file as your production template. It should contain:
- Track structure: A-roll, Screen/B-roll, Overlay, Audio, Music/Ambience
- Intro bumper (pre-built)
- Outro sequence with end screen placeholders
- Lower thirds style preset
- Color grade preset (if applicable)

**Per-video:** Duplicate the master template. Rename to video project file naming convention:
`YYYYMMDD_[topic-slug]_v1.tscproj`

---

## STAGE 4A — Recording: Talking Head + Screen Demo

> **This is Branch A.** Proceed to Stage 7 after recording.

### Setup
- Camtasia open with project file ready
- Screen content pre-loaded per pre-production checklist
- Camera, lighting, and mic active
- Performance script visible (second monitor or printed)

### Recording Protocol
1. Record **A-roll segments separately** from screen demos where possible — easier to edit
2. Slate each take verbally: *"Section 2, Take 1"* — makes timeline navigation faster
3. For mistakes: pause, take a breath, restart the sentence from the beginning — do not stop recording
4. Record screen demos in **Camtasia screen recorder** with system audio off unless intentional
5. Record a clean **room tone** clip of 10–15 seconds — useful for audio gap fills in editing

### File Output
Folder: `YYYYMMDD_[topic-slug]/`
- `aroll_[segment]_take[N].mp4`
- `screen_[segment].mp4`
- `assets/` (images, Canva exports, reference files)

---

## STAGE 4B — Recording: Talking Head / Green Screen

> **This is Branch B.** Proceed to Stage 7 after recording.

### Setup
- Green screen evenly lit — test chroma key in Camtasia before full recording session
- Background assets exported from Canva and ready in project folder
- Camera framing: leave headroom; avoid green-colored clothing or accessories

### Recording Protocol
1. Record A-roll segments with green screen active
2. Keep segments short — long takes on green screen increase keying complexity
3. Record a **clean plate** (5 seconds of green screen with no subject) at session start — useful for Camtasia keying reference
4. Slate takes verbally as above

### File Output
Same folder structure as Branch A.

---

## STAGE 4C — Recording: Slide-Driven Voiceover

> **This is Branch C.** Proceed to Stage 7 after recording.

### Setup
- Slide deck finalized and displayed full-screen
- Camtasia screen recorder active
- Mic tested and levels set

### Recording Protocol — Option 1: Live Narration
Record screen + voiceover simultaneously in Camtasia. Advance slides manually while speaking. Recommended for experienced narrators.

### Recording Protocol — Option 2: Segment-by-Segment
Record each slide or slide group as a separate take. Assemble in Camtasia timeline. Recommended for complex technical content.

### File Output
- `slides_[segment].mp4` (screen recordings)
- `voiceover_[segment].m4a` or `.wav` (if recorded separately)

---

## STAGE 7 — Edit & Assembly (Camtasia)

> All branches converge here.

### 7.1 Rough Cut
1. Import all recorded assets into Camtasia project (duplicate of master template)
2. Place clips in timeline in outline order
3. Trim: cut obvious mistakes, false starts, and dead air
4. Do not over-edit at this stage — get the story right first, then refine

### 7.2 Format-Specific Steps

**Branch A (Talking Head + Screen Demo):**
- Intercut A-roll and screen demo segments per Visual Intent notes in Content Brief
- Add zooms/pans on screen segments using Camtasia's built-in zoom tools to direct viewer attention
- Ensure clean audio match between A-roll and screen segments

**Branch B (Green Screen):**
- Apply Camtasia's Remove a Color (chroma key) to A-roll tracks
- Import Canva background assets and place on track below keyed footage
- Adjust key settings for clean edge — feather slightly to avoid hard outlines
- Preview composite before proceeding to overlays

**Branch C (Slide Voiceover):**
- Sync voiceover audio to slide recordings if recorded separately
- Add subtle zoom-in animations to slides (Camtasia Pan & Zoom) for visual interest
- Use callout/arrow overlays to direct attention to specific slide elements

### 7.3 Overlays & Polish
- Lower thirds: name/title at first appearance on camera
- Section labels: on-screen text at each new section (use Canva section card or Camtasia callout)
- Emphasis zooms: punch in on key moments in screen demos
- Callout boxes: highlight key data points or quotes

### 7.4 Audio
- Normalize audio levels across all clips
- Remove background noise using Camtasia's audio effects (or Audacity for problem clips)
- Add subtle background music under intro/outro (low volume, -20dB or lower under voice)
- Check: voice must remain dominant throughout

### 7.5 Intro & Outro
- Drop in pre-built intro bumper from master template
- Populate outro: add end screen links, subscribe prompt, and cards to related videos (YouTube cards added at upload — mark placeholder positions in Camtasia)

### 7.6 QC Review
- Watch at 1.25x speed in Camtasia preview
- Check: pacing, audio consistency, visual continuity, all overlay text correct
- If acceptable: export

### 7.7 Export Settings
- Resolution: 1920×1080 (standard) or 3840×2160 (4K if camera supports)
- Format: MP4, H.264
- Bitrate: 15–20 Mbps for 1080p; 40–50 Mbps for 4K
- File naming: `YYYYMMDD_[topic-slug]_v1.mp4`

---

## STAGE 8 — Metadata & Packaging (Channel-Specific)

> **Highest-leverage stage for discoverability.** Do not rush. Channel-specific requirements are defined in `channels/[channel-name].md`. The prompt templates below are pre-configured for YouTube. Adapt field labels for other channels.

### 8.1 Transcript Extraction
After export, generate or extract a transcript. Options:
- Upload to Claude and request a transcript cleanup
- Use YouTube's auto-caption after upload (download and clean)
- Use Camtasia's captioning tools

### 8.2 Metadata Package Generation — YouTube

```
LLM PROMPT — FULL METADATA PACKAGE (YOUTUBE)
----------------------------------------------
Here is the transcript/script for my YouTube video: [PASTE]

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

### 8.3 Metadata Package Review — Manual Step

AI generates; you validate. Check:
- Does the title sound like something you'd actually say?
- Does the description open with the viewer's problem, not your credentials?
- Are tags specific enough to matter, or are they too generic to rank?
- Do chapter titles make the video scannable in search previews?

Revise for voice before uploading anything.

### 8.4 Thumbnail Design (Canva)

**Process:**
1. Export 3–5 frame grabs from the video at key moments (Camtasia: Export → Single Frame)
2. Open your brand thumbnail template in Canva
3. Drop in the best frame grab as the background or focal image
4. Adjust text: maximum 3–5 words, large and legible at 120×67px (thumbnail preview size)
5. Export as PNG at 1280×720

**Canva Thumbnail Template Requirements (build once, reuse forever):**
- Brand colors applied
- Your name/logo in consistent position
- 1–3 large-text zones (headline, subline, optional label)
- High contrast between text and background
- Face visible if using A-roll or B-roll frame — eyes and expression matter for CTR

**AI Assist (optional):**
Use Canva Magic Design: upload your frame grab and let it generate 3–5 thumbnail layout variants. Select the strongest as a starting point, then apply your brand template layer over it.

**Test:** Create 2 thumbnail variants. A/B test using YouTube's built-in thumbnail test feature after publishing.

---

## STAGE 9 — Publishing Checklist

> Channel-specific publishing steps are defined in `channels/[channel-name].md`. The checklist below covers YouTube. Adapt for other channels by substituting the relevant channel configuration file.

### Pre-Upload
- [ ] Final QC watch complete (1.25x speed)
- [ ] Export file confirmed: correct resolution, naming convention applied
- [ ] Metadata package finalized: title, description, tags, chapters (YouTube) or equivalent
- [ ] Thumbnail(s) exported from Canva (2 variants for A/B test) — YouTube
- [ ] Idea Registry entry updated: status → `Published`
- [ ] Target Channel confirmed in Media Pipeline entry

### YouTube Upload
- [ ] Upload MP4 file
- [ ] Paste finalized title
- [ ] Paste finalized description (with placeholder timecodes replaced with actuals)
- [ ] Add tags
- [ ] Upload primary thumbnail
- [ ] Add chapter markers (verify timecodes match exported video)
- [ ] Set end screen elements (subscribe, related video links)
- [ ] Add cards at flagged positions from SEO Package
- [ ] Set category: Education or Science & Technology
- [ ] Language: English
- [ ] Schedule or publish

### Post-Publish (within 48 hours)
- [ ] Verify auto-captions are accurate — correct if needed or upload clean SRT
- [ ] Add to relevant YouTube playlist
- [ ] Share to LinkedIn (from Stage 10 repurposing output)
- [ ] Note publish date in Notion Video Pipeline tracker

---

## STAGE 10 — Analytics & Repurposing

### 10.1 Performance Tracking

Log metrics at **7 days** and **30 days** in Notion Video Pipeline (see Schema N2):

| Metric | Why it matters |
|---|---|
| CTR (Click-Through Rate) | Thumbnail + title effectiveness |
| Avg. View Duration | Content quality and pacing |
| Retention at 30 seconds | Hook effectiveness |
| Retention at 2 minutes | Intro/setup quality |
| Retention at 50% | Content depth and sustain |
| Total Views | Raw reach |
| Subscribers gained | Channel growth signal |

**Feed insights back:** High-performing hooks, thumbnail patterns, and topics go into your Notion Prompt Templates vault for future use. Underperforming elements get logged with a note in the Idea Registry.

### 10.2 Repurposing (AI-Assisted)

Triggered by the Repurposing Potential tags set in Stage 0.4.

```
LLM PROMPT — REPURPOSING PACKAGE
----------------------------------
Here is the transcript of my YouTube video: [PASTE]
Video title: [INSERT]
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

## COWORK INTEGRATION (Phase 2)

> Cowork is Anthropic's desktop AI agent — it reads, edits, and creates files in a designated local folder autonomously. Integrate as a workflow accelerator at two specific points once stable.

### Cowork Use Case 1 — Script Assembly
**Trigger:** After Stage 1 Content Brief is complete
**Action:** Point Cowork at your working video folder containing your Content Brief notes, rough outline, and reference documents. Instruct it to produce a first-draft performance script.
**Folder instruction to set:** *"You are assisting Dean, a Facilities Management professional and YouTube content creator. Audience is FM practitioners and CRE executives. Tone is authoritative, explanatory, and conversational. Always write speaking bullets, not prose. Flag technical jargon for plain-language alternatives."*

### Cowork Use Case 2 — SEO Package Assembly
**Trigger:** After video export, with transcript in project folder
**Action:** Point Cowork at the project folder containing the transcript and Content Brief. Instruct it to generate the full SEO package per the Stage 8 prompt template.

### Cowork Use Case 3 — Repurposing Package
**Trigger:** After publishing
**Action:** Point Cowork at the project folder with transcript. Generate blog, LinkedIn, newsletter, and Shorts scripts per Stage 10 prompt template.

**Prerequisites for Cowork integration:**
- Consistent local folder structure per video project (established in Stage 3)
- Global Cowork instruction set configured in Claude Desktop
- Folder-specific instructions file (`_COWORK_INSTRUCTIONS.md`) in each project folder

---

## MAKE AUTOMATION (Perplexity API Integration)

> Automates the SEO landscape scan (Stage 0.2 Step A) — triggered from Notion, results written back to Notion.

### Scenario Design

**Trigger:** Notion database item status changes from `Raw` → `Research Requested`

**Actions:**
1. Make reads the Working Title and Audience fields from the Notion Idea Registry entry
2. Make fires a Perplexity Sonar API call using the SEO Landscape Scan prompt template
3. Make writes the response back to the Notion Idea Registry entry in the Research Notes field
4. Make updates status to `Research Complete`

**Setup requirements:**
- Perplexity API key (Sonar model)
- Make Notion module with Idea Registry database connection
- Make HTTP module configured with Perplexity API endpoint
- Prompt template stored as a Make text variable

**Manual trigger alternative:** Until Make automation is configured, run the Perplexity prompt manually and paste results directly into Notion Research Notes field.

---

## NOTION SCHEMA

### N1 — Idea Registry Database

| Property | Type | Values / Notes |
|---|---|---|
| Idea Title | Title | Working title at capture |
| Status | Select | Raw, Research Requested, Research Complete, Validated, Needs Refinement, Abandoned, In Production, Published |
| Target Channel | Select | YouTube, LinkedIn Video, Podcast, Webinar, Other |
| Date Captured | Date | |
| Source / Trigger | Text | What prompted this idea |
| SEO Research Notes | Text | Perplexity output + manual platform findings |
| Differentiation Angle | Text | How this content differs from existing |
| Decision Rationale | Text | Why Accepted, Refined, or Abandoned |
| Repurposing Tags | Multi-select | Blog, LinkedIn, Newsletter, Short, Slide Deck, Podcast |
| Linked Media Project | Relation | → Media Pipeline database |
| Date Decided | Date | |
| Competing Content Found | URL | Links to top competitor content on target platform |

---

### N2 — Media Pipeline Database

| Property | Type | Values / Notes |
|---|---|---|
| Project Title | Title | Final working title |
| Status | Select | Scripting, Pre-Production, Recording, Editing, Packaging, Ready to Publish, Published |
| Target Channel | Select | YouTube, LinkedIn Video, Podcast, Webinar, Other |
| Format Branch | Select | A — Talking Head + Screen, B — Green Screen, C — Slide Voiceover |
| Target Length | Number | Minutes |
| Content Brief | URL / Relation | Link to Content Brief page |
| Performance Script | URL / Relation | Link to Script page |
| Camtasia Project File | Text | File path / naming |
| Export File | Text | File path / naming |
| Publish Date | Date | |
| Published URL | URL | Platform-specific link |
| 7-Day CTR | Number | % (YouTube) or equivalent platform metric |
| 7-Day Avg Duration | Number | Seconds |
| 30-Day Views | Number | |
| 30-Day Subscribers | Number | |
| Notes | Text | |
| Linked Idea | Relation | → Idea Registry |

---

### N3 — Prompt Templates Vault

A Notion page (not a database) organized by stage. Contains:
- All prompt templates from this system document (copy-paste ready)
- Annotated with what to customize per project and per channel
- Performance notes: which prompts have produced best results
- Version history: date of last update

**Structure:**
- Stage 0 Prompts: SEO/Competitive Landscape Scan, Differentiation Analysis
- Stage 1 Prompts: Title Finalization, Hook Development, Content Outline
- Stage 2 Prompts: Performance Script, Shorts Extraction
- Stage 8 Prompts: Full Metadata Package (YouTube), *(future: LinkedIn, Podcast)*
- Stage 10 Prompts: Repurposing Package
- Cowork Instructions: Global and folder-specific instruction templates
- Channel Config Reference: Links to `channels/` files in GitHub repo

---

### N4 — Performance Insights Log

A simple Notion page (updated periodically) capturing:
- **Hooks that worked:** Title patterns and opening lines from high-CTR videos
- **Thumbnail patterns that worked:** Visual style notes from high-CTR thumbnails
- **Topics that performed:** Subject matter, angle, and audience alignment notes
- **Topics that underperformed:** What didn't land and the likely reason
- **Prompt refinements:** What changes to prompt templates improved outputs

---

## QUICK REFERENCE — STAGE GATE CHECKLIST

| Stage | Input | Output | Tool |
|---|---|---|---|
| 0 — Idea Governance | Raw idea | Validated/Abandoned idea entry | Notion + Perplexity + LLM |
| 1 — Content Strategy | Validated idea | Content Brief | LLM + Notion |
| 2 — Scripting | Content Brief | Performance Script | LLM + manual review |
| 3 — Pre-Production | Performance Script | Asset checklist + Camtasia template | Canva + Camtasia |
| 4A/B/C — Recording | Pre-production package | Raw footage folder | Camtasia + camera |
| 7 — Edit & Assembly | Raw footage | Exported MP4 | Camtasia |
| 8 — Metadata & Packaging | Transcript + exported file | Metadata package + thumbnail | LLM + Canva |
| 9 — Publishing | Metadata package + file + thumbnail | Live published content | Channel platform |
| 10 — Analytics & Repurposing | Published content + transcript | Performance data + content derivatives | LLM + Notion |

---

## FILE & FOLDER NAMING CONVENTIONS

**Media project folder:**
`YYYYMMDD_[topic-slug]/`
e.g., `20260301_building-data-governance/`

**Sub-folders:**
```
20260301_building-data-governance/
├── aroll/
├── screen/
├── assets/
│   ├── canva-exports/
│   └── reference/
├── export/
└── _COWORK_INSTRUCTIONS.md
```

**Repository channel configuration folder:**
```
channels/
├── youtube.md          ← active v1.0
├── linkedin-video.md   ← planned
└── podcast.md          ← planned
```

**Export file:**
`YYYYMMDD_[topic-slug]_v1.mp4`

**Camtasia project:**
`YYYYMMDD_[topic-slug]_v1.tscproj`

**Thumbnail:**
`YYYYMMDD_[topic-slug]_thumb-A.png`
`YYYYMMDD_[topic-slug]_thumb-B.png`

---

*Media Production System v1.0 — Dean | 2026-02-21*
*Built for: AI-augmented solo creator workflow | FM & CRE subject matter expert*
*Initial channel: YouTube | Architecture: channel-agnostic*
