---
name: reverie
description: Generate, edit, and plan images, videos, and text using the Reverie MCP server (Seedream, Seededit, Seedance, Seed LLM). Routes to three sub-skills — `sd2-production-prompts` (per-clip Seedance 2.0 prompt template), `storyboard-maker` (visual stills + frame-chained video execution), and `content-pipeline` (six-stage production pipeline). For Seedance 2.0 video, recommends a 4-step pipeline — draft story → produce production prompt template → make storyboard stills → generate video. Use when the user asks to generate, create, edit, modify, or plan images, videos, or text with Reverie or ProjectReverie, or to build a storyboard / chain video clips into a continuous sequence. Adds a six-stage production pipeline (brainstorm → spec → plan → execute → review → report) with NodeFlow canvas execution for controlled multi-asset content work.
metadata:
  author: project-reverie
  version: "0.3.0"
---

# Reverie — AI Generation Skill

Generate images, videos, and text via the Reverie MCP server. This file is the **workflow router** — execution detail (parameter tables, model capabilities, constraints, tool reference) lives in `mcp-reference.md` in this same directory and is loaded only when generation actually begins. Heavy SD2 work and visual storyboarding live in sub-skills under `sub-skills/`.

**Pre-flight:** ensure the `reverie` MCP server is configured and connected before any tool calls.

---

## Sub-skills

Loaded on demand — do not read these unless the workflow routes to them.

| Sub-skill | Path | Purpose |
|-----------|------|---------|
| **sd2-production-prompts** | `sub-skills/sd2-production-prompts/SKILL.md` | Convert a creative brief into a paste-ready, per-clip Seedance 2.0 prompt template (4-section storyboard breakdown + compiled prompts with beats, audio, negatives, image-attachment numbering). Step 2 of the SD2 pipeline. |
| **storyboard-maker** | `sub-skills/storyboard-maker/SKILL.md` | Phase 1: render one Seedream 4.5 still per clip from each clip's BEAT 1 — the visual approval gate (Step 3). Phase 2 (optional): execute frame-chained video render with last-frame → first-frame chaining for continuous sequences. |
| **content-pipeline** | `sub-skills/content-pipeline/SKILL.md` | Six-stage controlled content production (superpowers-style); NodeFlow canvas as the granular execution surface. Stages are also exposed as /reverie:* slash commands via the companion plugin. |

---

## Modes

Decide which mode applies on the first message and route accordingly.

### Direct mode (default for clear generation requests)

Call the MCP tools directly. Use this mode when the user's request is concrete enough to generate immediately.

**Signals for direct mode:**
- Imperative + concrete subject: *"generate a sunset"*, *"make me a 6s video of waves"*, *"create a cyberpunk portrait at 16:9"*.
- Specific parameters mentioned (aspect ratio, duration, resolution, model name).
- An edit / iterate request on something just generated.
- Single-asset request, no campaign framing.

**To execute in direct mode:** read `mcp-reference.md` first for parameter tables, model capabilities, and constraints, then call the relevant MCP tool (`generate_image` / `generate_video` / `generate_text` / `understand_image`). No sub-skills, no approval gates.

**Iteration on an existing image visual** (e.g. *"try the same shot at sunset"*, *"regenerate, more dramatic"*, *"same prompt but cinematic mode"*) → use **`create_variant`**, not `generate_image`. It updates the visual in place and archives the previous prompt/asset into the visual's variations history, so the user doesn't end up with duplicates to clean up. See the `create_variant` section in `mcp-reference.md` for the parameter table and the "When to use which image tool" disambiguation between `generate_image`, `edit_image`, and `create_variant`.

### Production pipeline mode (controlled multi-asset work)

Route to `sub-skills/content-pipeline/SKILL.md` when the user asks for
campaign/multi-asset/controlled production work — e.g. *"produce a 3-shot
TikTok ad"*, *"run the full pipeline"*, *"brainstorm → spec → plan this
content"* — or invokes a `/reverie:*` command. One-shot requests stay
in direct mode.

**Precedence over SD2 mode:** when a request has campaign/multi-asset/controlled-production
framing, use Production pipeline mode even for Seedance-2.0 targets — the pipeline's Execute
stage delegates SD2 prompt/storyboard work to the SD2 sub-skills internally. Drop to bare SD2
pipeline mode only for a single SD2 video with no spec/plan framing.

**NodeFlow availability:** check `tools/list` once per session. If no
`nodeflow_*` tools are present, the pipeline still runs — execute with direct
tools and tell the user canvas features are unavailable on this server. When
they ARE present, read `nodeflow-reference.md` before any canvas work.

### SD2 video pipeline (recommended for Seedance 2.0 video work)

When the user wants to generate **video using Seedance 2.0** (model `2.0 Pro`, `2.0 Pro-Fast`, or any "SD2" / "Seedance 2" reference), default to the 4-step pipeline below rather than dropping into direct video generation. Each step is a review gate so the user can catch issues before paying compute for the next stage.

**Signals for the SD2 pipeline:**
- *"generate a video with SD2 / Seedance 2 / 2.0 Pro / 2.0 Pro-Fast"*.
- Multi-clip / sequence / brand-film / ad / scene-by-scene framing for an SD2 target.
- Anything where the brief is richer than a one-shot clip — even if the user didn't say "SD2" by name, route here when SD2 is the natural fit.

**Pipeline:**

| Step | Action | Where it lives | Output |
|------|--------|----------------|--------|
| 1 | **Draft the story** | This file (inline) | Plain-language treatment: synopsis, characters, key beats, mood, target duration. No prompt-engineering language yet. Stop and let the user revise. |
| 2 | **Produce production prompt template** | `sub-skills/sd2-production-prompts/SKILL.md` | `<project-slug>-sd2-prompts-compiled.md` — paste-ready per-clip Seedance 2.0 prompts. Run that sub-skill's Stage 1 (4-section breakdown) then Stage 2 (compiled prompts). |
| 3 | **Make storyboard (visual stills)** | `sub-skills/storyboard-maker/SKILL.md` Phase 1 | One Seedream 4.5 still per clip from each clip's BEAT 1, presented as a visual approval gate. Stop until every still is approved. |
| 4 | **Generate video with SD2** | This file (per-clip independent) **or** `sub-skills/storyboard-maker/SKILL.md` Phase 2 (chained) | The rendered MP4 clips. |

**Step 4 — choose the execution pattern:**
- **Per-clip independent (default for SD2 production work).** This file handles it: issue one `generate_video` per clip, passing `first_frame_url=firstFrameUrl[K]` (the approved storyboard still for clip K). Cuts are hard cuts. Visual cohesion comes from approved stills + prompts. Plan-governed concurrency caps still apply — serialize per the user's plan.
- **Frame-chained continuous sequence.** Route to `sub-skills/storyboard-maker/SKILL.md` Phase 2. Only clip 1 uses the approved still; clips 2..N start from the previous clip's last-frame poster. Use only when the user explicitly wants seamless continuity across cuts and accepts that storyboard stills 2..N were preview-only.

**When to skip the pipeline.** If the user explicitly says *"just generate the video"*, *"skip the storyboard"*, *"one-shot it"*, or asks for a single short clip with no production framing, drop to direct mode. Otherwise default to the 4-step flow for any SD2 video request.

### Frame-chain mode (existing multi-clip prompt file)

Use this mode when the user already has a multi-clip prompt file (typically a Seedance shot list — output of `sd2-production-prompts` from a prior session, or hand-written) and wants the rendered clips to flow continuously without going back through Steps 1–3.

**Signals:**
- *"chain these clips"*, *"connect these video clips"*, *"continuous sequence"*, *"render this storyboard as one flowing video"*.
- A markdown / text file attached or referenced (e.g. `@some-prompts.md`) containing multiple clip prompts plus a request to render them as a chain.

**To execute:** route to `sub-skills/storyboard-maker/SKILL.md` and run **Phase 1 (storyboard stills)** if the user wants a visual approval gate, then **Phase 2 (frame-chained video execution)**. If the user explicitly skips Phase 1 (*"just chain them, no preview"*), Phase 2 still needs a clip-1 first-frame still — generate it inline as that sub-skill describes.

### Mode can switch mid-conversation

If the user hands over a multi-clip prompt file mid-conversation and wants it rendered, switch to frame-chain mode (or the SD2 pipeline if they're starting from a brief, not a prompt file).

---

## Product Naming — always use **ProjectReverie**

The product's full name is **ProjectReverie** (one word, capital P and R). When referring to where a generation was saved or where the user will see it, say **ProjectReverie** — not "Studio", not "Reverie", and **never** the compounded form "Reverie Studio" / "ReverieStudio".

- ✅ "Saved in ProjectReverie under your General project."
- ❌ "Saved in Reverie Studio…"
- ❌ "Saved in Studio…"
- ❌ "Saved in Reverie…"

Internal names that still appear in code/docs (do not surface these to the user as the product name):
- `reverie` — MCP server identifier (tool namespace only).
- "Studio" / "Recent Projects" — legacy UI labels inside ProjectReverie.
- `project-reverie` — repo/workspace slug.

## Terminology: "folder" means **Project**

The Studio UI shows generations grouped under **"Recent Projects"** (e.g. `General`). In this skill's context:

- **"folder"**, **"project"**, and **"collection"** all refer to the same thing: a **Studio project** — the destination where generated images and videos are saved.
- Never create a **filesystem folder** when the user says "folder" in a Reverie/Studio context. The only exception is if the user explicitly says "on disk", "local folder", or provides a filesystem path.
- To create or list projects, use the MCP tools `create_project` / `list_projects` (documented in `mcp-reference.md`). To target a project when generating, pass `project_name` to `generate_image` / `generate_video`.

> The backend API route is still `/api/v1/folders` (legacy naming) but the user-facing surface — UI labels, MCP tool names, and skill vocabulary — is **Project**. Match the UI when speaking to the user.

---

## Prompt Engineering References

Deep prompt-crafting guides are bundled with this skill under `prompts-guide/`. **Do not load these by default** — they're large. Read the file that matches the user's model or task only when the user asks for prompt help, examples, or advanced techniques.

| When the user is… | Read |
|-------------------|------|
| Asking for general prompt tips (any model) | `prompts-guide/prompt-engineering-best-practices.md` |
| Using Seedream image models (4.0 / 4.5) | `prompts-guide/seedream-4.5.md`, then `prompts-guide/seedream-4.0-4.5-tutorial.md` for worked examples |
| Generating video with Seedance 2.0 | `prompts-guide/seedance-2.0-prompt-guide.md`, plus `prompts-guide/seedance-2.0-tutorial.md` for scene breakdowns |
| Generating video with Seedance 1.5 Pro | `prompts-guide/seedance-1.5-pro.md` |
| Generating video with Seedance 1.0 Pro / Pro-Fast | `prompts-guide/seedance-1.0-pro.md` |
| Asking "how do I prompt video" in general | `prompts-guide/video-generation-tutorial.md` |

Rule of thumb: pick the narrowest-matching file first. Only load a second file if the first doesn't answer the question.
