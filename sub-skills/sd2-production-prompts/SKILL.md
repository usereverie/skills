---
name: sd2-production-prompts
description: Sub-skill of `reverie`. Produces shot-by-shot Seedance 2.0 video prompts from a creative brief — Stage 1 emits a 4-section storyboard breakdown (effects timeline, master inventory, density map, energy arc); Stage 2 compiles paste-ready per-clip prompts with image-attachment numbering, beat structure, per-beat audio, and negative prompts. Loaded by `reverie/SKILL.md` as Step 2 of the SD2 video pipeline. Do not invoke standalone — the parent `reverie` skill routes here.
---

# Reverie sub-skill — SD2 Production Prompts

Build cinematic, shot-by-shot video prompts from a creative brief. Every output follows a structured effects breakdown format designed to give Seedance 2.0 maximum detail on camera work, effects, transitions, pacing, and energy arc.

This is **Step 2 of the SD2 video pipeline** described in `../../SKILL.md`. The story draft (Step 1) is the input; the compiled per-clip prompt file is the output and feeds the storyboard-maker sub-skill (Step 3).

## How this sub-skill works

It operates in **two stages**. Always run Stage 1 first when given a brief; only run Stage 2 when the user explicitly asks to compile or has confirmed the Stage 1 output.

### Stage 1 — Storyboard (default)

The 4-section structured breakdown — shot-by-shot timeline, master effects inventory, density map, energy arc. This is the **creative direction layer**: it tells you what to shoot, what effects to stack, and how the energy arc flows. It is NOT paste-ready as a Seedance 2.0 `prompt` parameter.

**Trigger:** any new creative brief.

**Steps:**
1. The user provides a **creative brief** — this can be as simple as "a runner in a stadium for a Nike-style ad" or as detailed as a full storyboard description. They may also provide a reference video, mood, brand context, or specific effects they want.
2. Read the reference file at `references/effects-breakdown-reference.txt` to internalise the structure and level of detail expected.
3. Generate a complete storyboard in plain text, structured into the four mandatory sections below.
4. Stop. Wait for the user to review/revise OR ask to compile.

### Stage 2 — Compile Production Prompts (on request)

A paste-ready markdown file with per-clip Seedance 2.0 prompts that you can copy directly into the `prompt` parameter of `mcp__reverie__generate_video`. This includes image-attachment numbering, beat structure, per-beat audio, and negative-prompt blocks.

**Triggers:** the user asks to "compile", "make paste-ready", "production prompts", "SD2 final prompt", "compile clip prompts", or has approved a Stage 1 storyboard and asked to proceed.

**Steps:**
1. Read `references/production-prompt-template.md` to internalise the production format and the worked example.
2. Map each storyboard shot/clip onto a per-clip block in the compiled file.
3. For every beat, apply the **Per-Clip Prompt Engineering Principles** (defined later in this file).
4. If the user has reference images, build the **Image Attachment Key** table and reference each `[IMG-N]` per clip.
5. Save to `<project-slug>-sd2-prompts-compiled.md` (or output in chat if the user prefers).

## Input expectations

The user's brief can include any combination of:
- Subject/talent description (who or what is on screen)
- Setting/environment
- Mood, tone, energy level
- Brand or product context
- Specific effects or camera moves they want
- Duration target
- Reference to existing ads, films, or visual styles
- Colour palette or grade preferences

If the brief is too vague to build a full prompt (e.g. "make something cool"), ask one focused clarifying question before proceeding. Don't over-interrogate — work with what you're given and make creative decisions where the user hasn't specified.

## Stage 1 Output — Storyboard

ALWAYS output ALL FOUR sections in this exact order. Never skip a section.

### Section 1: SHOT-BY-SHOT EFFECTS TIMELINE

This is the core of the prompt. Each shot gets its own block structured like this:

```
SHOT [N] ([timestamp]) — [Shot Name / Description]
• EFFECT: [Primary effect name] + [secondary effects if stacked]
• [Detailed description of what's happening visually]
• [Camera behaviour — angle, movement, lens if relevant]
• [Speed/timing information]
• [How this shot connects to the next — transition type]
```

Guidelines for writing shots:
- Each shot should be 1-4 seconds unless the brief calls for longer holds
- Name effects precisely: "speed ramp (deceleration)" not just "speed ramp"; "digital zoom (scale-in)" not just "zoom"
- Describe stacked effects explicitly — if 3 things happen at once, list all 3
- Include transition logic: how does this shot EXIT and how does the next shot ENTER?
- Use language Seedance 2.0 can interpret: describe the visual result, not the editing software technique. For example, say "the frame scales inward rapidly" rather than "apply a keyframed scale effect in After Effects"
- Note the most impactful or signature shot with a callout like "This is the SIGNATURE VISUAL EFFECT"
- Be specific about speed percentages when using slow-motion (e.g. "approximately 20-25% speed")
- Describe motion blur, light behaviour, and atmospheric effects where relevant

### Section 2: MASTER EFFECTS INVENTORY

A numbered list of every distinct effect used across the full prompt, with:
- Effect name
- How many times it's used (e.g. "used 3x")
- Which shots it appears in
- A one-line description of its role in the edit

This section helps the user (and the generator) see the full palette of techniques at a glance. Group similar effects together. Typical categories include: speed manipulation, camera movement, digital effects, transitions, compositing, optical effects.

### Section 3: EFFECTS DENSITY MAP

Break the timeline into segments (roughly 3-6 second chunks) and rate each as:
- **HIGH DENSITY** — 4+ effects stacked or rapid-fire
- **MEDIUM DENSITY** — 2-3 effects
- **LOW DENSITY** — 1 effect or clean/simple footage

Format:
```
[timestamp range] = [DENSITY LEVEL] ([brief list of effects] — [count] effects in [duration])
```

### Section 4: ENERGY ARC

Describe the overall energy structure of the video as a narrative arc. The reference uses a three-act model:
- **Act 1**: Opening energy — how the video grabs attention
- **Act 2**: Middle section — how it develops and what the signature moments are
- **Act 3**: Resolution — how the energy resolves and lands

Adapt the number of acts to suit the video's length and structure. A 5-second clip might only need two beats; a 30-second brand film might need four.

## Stage 2 Output — Compiled Production Prompts

When the user triggers Stage 2, produce a paste-ready markdown file. The format is the same one used in Reverie's reference projects — designed to be copied directly into `mcp__reverie__generate_video` calls.

### File format

Single markdown file, suggested name `<project-slug>-sd2-prompts-compiled.md`. If the user wants both an annotated working doc and a clean compiled doc, also produce `<project-slug>-sd2-prompts.md` with version history, editing notes, and source-story map. Default to the compiled-only file unless asked otherwise.

### Top of the file

- **Title:** `# <Project Title> — Compiled Clip Prompts`
- **Settings line:** one paragraph immediately under the title naming the global Seedance settings the prompts target. Example: ``Settings: `9:16 · 480p · audio_sync=true · model 2.0 Pro-Fast · reference mode`.`` Match whatever the user specified, or recommend defaults if they didn't.
- **Image Attachment Key table** (only if the user has reference images — see "Image Attachment Convention" below).

### Per-clip blocks

Each clip is a section. Format:

````
## Clip <letter>

**Attach references:** `[IMG-X]` + `[IMG-Y]` + …

```
<full prompt body>
```
````

### Clip prompt body structure

Inside each fenced code block, the prompt body has four parts in this order:

1. **Style preamble** — one sentence naming aspect ratio, art direction, and color palette. Example: *"A vertical 9:16 animated cinematic shot, modern graphic novel aesthetic with bold black ink outlines, halftone shading, painterly textures, warm color palette."*
2. **Beat blocks** — `BEAT N — TITLE (0:00–0:0X):` per beat, one paragraph each. Each beat names its visual subject, framing, action, and timing.
3. **Audio paragraph** — one paragraph starting with `Audio:`. Tie sound design to specific beats. Quote dialogue verbatim and name the on-screen visual during the line.
4. **Negative prompts line** — one line starting with `NEGATIVE PROMPTS:`. Comma-separated list of failure modes specific to this clip.

See `references/production-prompt-template.md` for a worked example.

## Image Attachment Convention

When the user has reference images (typically from prior `/reverie` `generate_image` calls or external uploads), use this numbering convention so the compiled prompts cross-reference cleanly:

- **Numbering:** `[IMG-1]`, `[IMG-2]`, `[IMG-3]`, … — start with the hero/style poster (if any), then character sheets, then any scene/style references.
- **Key table** at the top of the compiled file:

  ```
  | # | Asset | Use as reference for | URL |
  |---|---|---|---|
  | **[IMG-1]** | Hero poster — <name> | Overall visual style, color palette | <url> |
  | **[IMG-2]** | Character sheet — <name> | <character> in all clips they appear | <url> |
  ```

- **Per-clip attachment line:** above each clip's prompt block, name which images to attach: `**Attach references:** [IMG-1] + [IMG-2] + [IMG-4]`.
- **Limits:** Reference mode (`reference_image_urls`) is supported by the Seedance 2.0 family — `2.0 Pro`, `2.0 Pro-Fast`, and `2.0 Mini` all accept up to **9 reference images** per clip (`2.0 Pro` / `2.0 Pro-Fast` also take video/audio refs on the canvas). 1.x models (`1.5 Pro`, `1.0 Pro`, `1.0 Pro-Fast`) don't support reference mode.
- **Frame mode vs reference mode:** these are mutually exclusive per request. The compiled prompts default to **reference mode** (model uses images as style/character anchors). Use **frame mode** (`first_frame_url` / `last_frame_url`) only when the user is intentionally chaining clips with locked first/last frames.
- **Asset URLs:** include the full public URL in the key table. Optionally also include a code block with the URLs separated by newlines, suitable for paste into the MCP `reference_image_urls` array.

## Creative principles

These principles should guide every prompt you write:

1. **Contrast drives impact.** Alternate high-density and low-density moments. A slow-motion shot after a speed ramp hits harder than two speed ramps back-to-back.
2. **Signature moments matter.** Every video should have at least one "hero" effect — something visually distinctive that makes it memorable. Call it out explicitly.
3. **Transitions are shots.** Don't treat transitions as throwaway connectors. A whip pan, a bloom flash, a motion blur smear — these are creative moments, not just cuts.
4. **Specificity over vagueness.** "The frame rotates clockwise by approximately 15-20°" is better than "the camera tilts." "Approximately 20-25% speed" is better than "slow motion."
5. **Energy must resolve.** No matter how intense the opening, the video needs to land. The final moments should feel intentional, not like the effects budget ran out.

## Per-Clip Prompt Engineering Principles

These principles apply to **Stage 2 only** — the per-clip Seedance 2.0 prompts. They are the lessons from real production runs where v1 prompts produced wrong imagery and v2 rewrites fixed it. Apply every one to every beat.

1. **Beat structure.** Every clip is divided into explicit beats with timestamps: `BEAT N — TITLE (0:00–0:0X):`. One paragraph per beat. Naming the beat helps the model anchor the visual logic. A beat without a name often gets visually merged with the next.

2. **Subject clarity.** Name the PRIMARY focal point AND what is secondary. Vague subject = AI defaults to dramatic/oversized. Example: *"The PAGE is the subject — paper texture, faint blue ruled lines, spiral binding visible. The tear is a tiny detail, NOT the focus."*

3. **Scale specificity.** Use real-world measurements: mm, cm, degrees, percent. Example: *"3-4mm, the natural size of a real human tear, NOT a giant droplet."* Without explicit scale, the model picks dramatic by default — small things become massive.

4. **Camera framing precision.** Name the framing class explicitly and include negative framing where useful. *"Over-the-shoulder mid-shot"*, *"low-angle child's-eye-level push-in"*, *"NOT macro close-up"*, *"mid-shot framing with shallow depth so the card is sharp and the coat is soft behind"*. Negative framing instructions matter as much as positive ones.

5. **Action verb precision.** Replace metaphorical verbs with concrete ones. *"Draws ITSELF into a coherent comic illustration"* beats *"spreads"*. *"PURPOSEFUL strokes, not chaotic spreading"* beats *"organic growth"*. Metaphorical verbs invite the model to free-associate to genre tropes (often horror).

6. **Per-beat audio.** Sound design must be tied to specific timing. Don't lump audio at the end. Example: *"Audio: solo piano holding a single note across Beat 1, sharp glass-shatter impact on Beat 2, ambient lobby tone with low murmur on Beat 3."*

7. **Dialogue placement.** When a line is spoken, name the visual on screen during that line. Example: *"Elena's whispered voice says: '...you drew this?' The line is delivered while the camera holds on the artwork."* Without this, the model often pairs the audio with the wrong visual (e.g. cuts to a different shot mid-line).

8. **Negative prompts.** Every clip ends with a `NEGATIVE PROMPTS:` line listing the specific failure modes most likely for THIS clip. Generic bans don't help. Write them after thinking through what could go wrong. Example: *"NEGATIVE PROMPTS: no giant water droplets, no ink vines, no ink tentacles, no ink roots, no organic growth patterns, no horror imagery."*

9. **Banned trigger words.** Certain words reliably produce the wrong imagery. Avoid these unless that's literally what you want:
   - **"vines / roots / tentacles / growth / spreading / blooming"** → the model produces horror tendrils. Use *"draws itself"*, *"forms"*, *"completes"*, *"resolves into"* instead.
   - **"macro extreme close-up"** of a small object → makes the object massive (filled the frame as the literal subject). Use *"wide shot of [larger context], [small object] is a tiny detail"* instead.
   - **"the [small object] is the subject"** + macro framing = same problem. Be explicit about what's primary vs secondary in framing.
   - **Generic "epic" / "dramatic" / "cinematic" alone** → invites default Hollywood action staging. Combine with concrete framing, lighting, and lens info.

10. **Iteration pattern (v1 → v2).** Treat the first render as a draft. When something fails, identify the root cause in the prompt:
    - Vague subject? → add explicit primary/secondary clarification.
    - Trigger word? → replace with a concrete verb + add to NEGATIVE PROMPTS.
    - Lumped beats? → split into separate beat blocks with timestamps.
    - Dialogue/visual mismatch? → name the on-screen visual during the line.

    Document the diff in a "Key changes v1 → v2" table inside the annotated working doc (if produced). Keep the v1 render link as a reference, not as the deliverable.

## Tone and style

- Write in a direct, technical tone — like a director's shot notes, not a marketing brief
- Use bullet points within each shot block for clarity
- Be concise but complete — every detail should earn its place
- No hype language, no "stunning" or "breathtaking" — describe what happens and let the visuals speak

## Duration calibration

Adjust the number of shots and effects density to match the target duration:
- **5-10 seconds**: 4-7 shots, lean and punchy, 1 signature effect
- **10-20 seconds**: 8-14 shots, room for contrast and build, 1-2 signature effects
- **20-30 seconds**: 12-20 shots, full three-act arc, 2-3 signature effects
- **30+ seconds**: Scale accordingly, but maintain density contrast — don't fill every second with effects

If the user doesn't specify a duration, default to 15-20 seconds (a sweet spot for AI video generation).

## Example workflow

### Single-stage (storyboard only)

**User says:** "I want a dramatic brand film for a trail running shoe. Mountain setting, golden hour, single runner. Make it feel epic but not over-the-top. About 15 seconds."

**You do (Stage 1):**
1. Read `references/effects-breakdown-reference.txt` to calibrate detail level.
2. Generate the full four-section output: shot-by-shot timeline (8-12 shots), master effects inventory, density map, and energy arc.
3. Present in plain text in chat. Stop.

### Two-stage (storyboard → compiled production prompts)

**User says (turn 1):** "I want a 26s inspirational TikTok video about an artist who keeps getting rejected, then breaks through."

**You do (Stage 1):**
1. Read `references/effects-breakdown-reference.txt`.
2. Generate the 4-section storyboard. Present in chat. Stop.

**User says (turn 2):** "Compile production prompts. Use these reference images: [hero poster URL], [character sheet URL]."

**You do (Stage 2):**
1. Read `references/production-prompt-template.md` for the production format and worked example.
2. Map each storyboard shot/clip onto a per-clip block. Group adjacent shots that share location/character into single 4–7s clips (Seedance 2.0 min duration is 4s, max is 12s).
3. For every beat, apply the **Per-Clip Prompt Engineering Principles**: beat structure, subject clarity, scale specificity, camera framing precision, action verb precision, per-beat audio, dialogue placement, negative prompts, banned trigger words.
4. Build the **Image Attachment Key** table from the user's reference image URLs. Number them `[IMG-1]`, `[IMG-2]`, … starting with the hero/style poster, then character sheets.
5. For each per-clip block, choose which `[IMG-N]` to attach (max 9 for 2.0 Pro/Pro-Fast).
6. Save the result to `<project-slug>-sd2-prompts-compiled.md`. If the user wants the annotated working doc too, also save `<project-slug>-sd2-prompts.md` with version history and editing notes.
