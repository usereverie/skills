---
name: storyboard-maker
description: Sub-skill of `reverie`. Renders the visual storyboard for a multi-clip video plan — one Seedream 4.5 still per clip from the clip's BEAT 1 description — and optionally executes the frame-chained video render (each clip's last-frame poster becomes the next clip's first frame). Loaded by `reverie/SKILL.md` as Step 3 (and the chained-execution variant of Step 4) of the SD2 video pipeline. Do not invoke standalone — the parent `reverie` skill routes here.
---

# Reverie sub-skill — Storyboard Maker

Two phases:

1. **Storyboard phase** — render one still image per clip so the user can approve the visual direction *before* spending compute on video generation.
2. **Chained-execution phase (optional)** — render the actual video clips with last-frame → first-frame chaining, so cuts share visual state and the sequence plays as one continuous piece.

Phase 1 is the cheap quality gate. Phase 2 is one of two valid execution patterns for Step 4 of the SD2 pipeline (the other is per-clip independent renders, which `reverie/SKILL.md` handles directly).

**Pre-flight:** the `reverie` MCP server must be configured and connected. Read `../../mcp-reference.md` once for parameter tables before any tool calls.

---

## Phase 1 — Storyboard (visual stills)

### When to run

After the SD2 production-prompt template is approved (Step 2). Before any `generate_video` call. If the user explicitly says *"skip the storyboard"* / *"just render the videos"*, skip Phase 1 entirely and tell `reverie/SKILL.md` to drop into direct video generation.

### Inputs

- A multi-clip prompt file (typically `<project-slug>-sd2-prompts-compiled.md` from the sd2-production-prompts sub-skill). Each clip must have a `BEAT 1 — …` block.
- Optional: reference images (style poster, character sheets) already declared in the prompt template's Image Attachment Key.
- Optional: a Studio project name to bucket the stills under.

### Steps

1. **Pre-flight.** Call `get_credit_balance`. If 0, stop and tell the user to top up. Optionally `create_project(name="<source-stem>-storyboard")` so all stills + downstream videos land in one project.
2. **Extract BEAT 1 per clip.** For each clip K:
   - Pull the BEAT 1 paragraph from the prompt template.
   - Strip temporal verbs and pacing language: *"slow push-in"*, *"creeping forward"*, percentages (*"20-25% speed"*), time codes (*"0:00–0:03"*), beat labels. Keep static subject, framing, lighting, lens, color palette, character/prop description.
   - Prepend the clip's style preamble (aspect ratio, art direction) so the still matches the video's visual language.
3. **Render still K.** Call `generate_image(model="4.5", resolution="2K", aspect_ratio=<source>, aesthetic_mode="cinematic", prompt=<stripped BEAT 1>, reference_image_urls=<style + character refs declared for clip K>, project_name=<storyboard project>)`. Poll until `succeeded`. Capture `asset_url` as `firstFrameUrl[K]`.
4. **Repeat sequentially or in parallel.** Stills are independent — parallel kickoff is fine, subject to plan-governed concurrency caps. Don't fan out beyond what the user's plan allows.
5. **Present the storyboard for approval.** Output a markdown table or inline image gallery:

   ```markdown
   # <Title> — Visual Storyboard

   | Clip | BEAT 1 (visual prompt) | Still |
   |------|------------------------|-------|
   | A    | <one-line summary>     | ![A](<asset_url>) |
   | B    | <one-line summary>     | ![B](<asset_url>) |
   | …    |                        |                   |
   ```

   Stop. Wait for the user to approve, request revisions, or ask to proceed to video.

6. **Revision loop.** If the user wants a still re-rendered, edit the stripped BEAT 1 (or use `edit_image` with the existing still as input + a focused instruction) and replace `firstFrameUrl[K]`. Don't proceed to Phase 2 until every still is approved.

### Output of Phase 1

A `firstFrameUrl[K]` for every clip K, plus the visual storyboard markdown. These URLs feed Step 4 of the SD2 pipeline:

- **Per-clip independent execution** (default for SD2 production work) — `reverie/SKILL.md` handles this directly: it issues one `generate_video` per clip, passing `first_frame_url=firstFrameUrl[K]` for each. Cuts are hard cuts; visual cohesion comes from the approved stills + prompts.
- **Frame-chained execution** — call Phase 2 below if the user wants seamless continuity across cuts.

---

## Phase 2 — Frame-chained video execution (optional)

### When to run

The user explicitly wants continuous-sequence rendering — *"chain these clips"*, *"continuous sequence"*, *"no jump cuts"*, *"render as one flowing video"*. Otherwise prefer per-clip independent execution (handled by `reverie/SKILL.md`).

### Inputs

- The approved `firstFrameUrl[1]` from Phase 1 (clip 1's storyboard still). Clips 2..N's first frames will be derived from the previous clip's last-frame poster — the storyboard stills 2..N were preview-only and are not used as `first_frame_url` here.
- The full multi-clip prompt file with each clip's prompt body, duration, and `Audio:` block.

### Generation contract — SEQUENTIAL ONLY

This is load-bearing. **Violating it breaks frame continuity.**

1. **One video at a time.** Clip K+1 cannot start until clip K returns `succeeded` with a non-null `poster_url`.
2. **Never batch parallel video calls.** Even if the user asks for speed, refuse and explain the chain dependency.
3. **Always pass `first_frame_url` to clips 2..N.** The URL is clip K-1's `poster_url`.
4. **Never combine `first_frame_url` with `reference_image_urls`** — backend rejects mixed payloads.

### Steps

For `k = 1..N`, in order:

1. Build the call: `generate_video(prompt=<clip k full body>, model=<sd2 model — "2.0 Pro" or "2.0 Pro-Fast" by default for this pipeline; "1.5 Pro" if user asked>, resolution=<as specified>, aspect_ratio=<source>, duration=<clip k>, audio_sync=true, first_frame_url=<firstFrameUrlForK>, project_name=<storyboard project>)`.
   - For `k == 1`: `firstFrameUrlForK` = `firstFrameUrl[1]` from Phase 1.
   - For `k > 1`: `firstFrameUrlForK` = the `poster_url` returned for clip `k-1`.
2. Poll `check_generation_status(task_id)` until `status == "succeeded"`. Pause 10–20s between polls — don't spam.
3. Capture `poster_url`. Store as `lastFrameUrl[k]`.
4. Only then proceed to `k+1`.

If a clip fails: surface the error, offer to retry that single clip (default) or stop the chain. Never silently skip — clip k+1 needs `lastFrameUrl[k]`.

### Step 3 — Compile the storyboard report

Default filename: `<source-stem>-storyboard.md` in the user's working directory. Structure:

```markdown
# <Title> — Storyboard

Generated from [<source-file>](<source-file>).
First frame: Seedream 4.5 (BEAT 1 still). Last frames: Seedance video posters.
Total runtime: <sum>s.

| # | Clip | Duration | Dialog | First frame | Last frame |
|---|------|----------|--------|-------------|------------|
| 1 | <label>          | Xs | <dialog or "*(no dialogue — SFX summary)*"> | [first](<image url>) | [last](<poster url>) |
| 2 | <label>          | Xs | …                                           | —                    | [last](<poster url>) |
| … |                  |    |                                             |                      |                      |
| N | <label>          | Xs | …                                           | —                    | [last](<poster url>) |
```

Table rules:
- Only **row 1** carries a `First frame` URL — the Seedream-generated still.
- Every row carries a `Last frame` URL — the `poster_url` from `check_generation_status`.
- Rows 2..N: `First frame` cell is the literal `—` (the first frame *is* the previous row's last frame; treat as implicit).
- `Dialog` cell: spoken lines as `Speaker (descriptor): "line"`; dialogue-free clips get an italicised SFX summary, e.g. `*(no dialogue — piano + glass shatter SFX)*`. On-screen text appended after a semicolon: `; on-screen text: "…"`.

### Step 4 — Report to the user

Tell the user: where the storyboard markdown was written, the ProjectReverie project name holding the source MP4s + still, total runtime, and any clips that needed retries.

### Breaking the sequential rule

Only one case: the user explicitly opts out of frame continuity (*"just batch them, I don't care if cuts are jumpy"*). In that case, switch to parallel kickoff but warn once that cuts won't share visual state. Default behaviour stays sequential.
