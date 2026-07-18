# Production Prompt Template — Stage 2 Reference

A generalized template for producing paste-ready Seedance 2.0 clip prompts (Stage 2 output). Read this before compiling. The fully-worked example clip at the bottom shows what a finished beat block looks like end-to-end.

---

## How to fill this template

1. **Plan clip count and duration first.** Aim for clips of 4–7 seconds each. Seedance 2.0 minimum is 4s, maximum is 12s. A 25-second video typically maps to 4–5 clips.
2. **Apply every Per-Clip Prompt Engineering Principle** from `SKILL.md` to every beat — beat structure, subject clarity, scale specificity, camera framing precision, action verb precision, per-beat audio, dialogue placement, negative prompts, banned trigger words.
3. **Always specify `audio_sync` explicitly** in the global settings line. Default to `true` unless the user wants silent. Audio doubles per-clip cost on 2.0 Pro / 2.0 Pro-Fast / 1.5 Pro.
4. **TikTok defaults:** `9:16` aspect, `480p` or `720p` resolution — `720p` is the practical ceiling for `2.0 Pro-Fast` / `2.0 Mini` (both cap there regardless of reference images); `2.0 Pro` also supports `1080p` / `4K` for higher fidelity. Reference images do not restrict resolution — confirm the chosen model's ceiling via `list_models`.
5. **Reference vs frame mode:** default to **reference mode** (`reference_image_urls`). Use **frame mode** (`first_frame_url` / `last_frame_url`) only if the user explicitly wants clips chained on locked frames — these are mutually exclusive per request.
6. **Image attachment numbering:** see the Image Attachment Convention section in `SKILL.md`. Number `[IMG-1]` starting with the hero/style poster, then character sheets, then scene refs.

---

## Template structure

````markdown
# <Project Title> — Compiled Clip Prompts

Paste-ready Seedance 2.0 clip prompts for *<Project Title>*. Settings: `<aspect> · <resolution> · audio_sync=<true|false> · model <2.0 Pro | 2.0 Pro-Fast | 1.5 Pro> · <reference|frame> mode`.

---

## Image Attachment Key

| # | Asset | Use as reference for | URL |
|---|---|---|---|
| **[IMG-1]** | <hero / style poster> | Overall visual style, color palette | <url> |
| **[IMG-2]** | <character sheet — protagonist> | <protagonist> in all clips they appear | <url> |
| **[IMG-3]** | <character sheet — antagonist or supporting> | <character> in clips they appear | <url> |
| **[IMG-N]** | … | … | … |

**Asset URLs (paste into the MCP `reference_image_urls` array):**
```
[IMG-1] <url>
[IMG-2] <url>
[IMG-N] …
```

---

## Clip A

**Attach references:** `[IMG-1]` + `[IMG-2]`

```
A vertical 9:16 animated cinematic shot, <art direction>, <color palette>.

BEAT 1 — <BEAT TITLE> (0:00–0:0X): <Wide / medium / close framing>. <Subject description naming the PRIMARY focal point>. <Setting and lighting>. <Camera behavior with concrete numbers — speed %, degrees, distance, depth-of-field>. <Secondary detail with NEGATIVE clarification of what it is NOT>.

BEAT 2 — <BEAT TITLE> (0:0X–0:0Y): <Transition method from Beat 1 — match cut, cross-dissolve, hard cut through impact, etc.>. <Action description with PURPOSEFUL verb choices, not metaphorical ones>. <Scale specifics — mm, cm, percent, degrees>.

BEAT N — <BEAT TITLE> (0:0Y–0:0Z): <…>

Audio: <Beat-by-beat sound design — name what plays during each beat with timing markers. If dialogue, quote the line verbatim and specify which visual is on screen during it. Example: "Solo piano holding a note across Beat 1, sharp glass-shatter impact on Beat 2 transition, ambient lobby tone on Beat 3. <Speaker>'s voice (whispered, warm) says: '<line>' delivered while the camera holds on <visual>.">.

NEGATIVE PROMPTS: <comma-separated list of specific failure modes to ban — banned trigger words, oversized objects, wrong tone, wrong era, wrong genre, etc.>.
```

---

## Clip B

**Attach references:** `[IMG-1]` + `[IMG-2]` + `[IMG-3]`

```
<same structure>
```

---

[continue for Clip C, D, …]
````

---

## Worked example clip

This is what a finished `Clip` block looks like end-to-end, using anonymized placeholders. Treat it as a quality bar, not as content to copy literally — your beats should be specific to the user's project, not these placeholders.

````markdown
## Clip C (Worked Example) — "Pivot Moment" — SIGNATURE SHOT (7s)

**Attach references:** `[IMG-1]` + `[IMG-2]` + `[IMG-3]`

```
A vertical 9:16 animated cinematic shot, modern graphic novel aesthetic with bold black ink outlines, halftone shading, painterly textures.

BEAT 1 — <SMALL OBJECT ON SURFACE> (0:00–0:01): WIDE shot of <a flat surface — page, table, floor> filling most of the frame, lit by a single warm <light source>. The <SURFACE> is the subject — <texture, color, detail>, NOT a macro of the small object. A single REALISTIC small <object> (3-4mm, the natural size of a real <object>, NOT a giant/oversized version) sits in the lower-third of the frame. The small object is a tiny detail, NOT the focus. Camera static, slight shallow depth of field on the surface.

BEAT 2 — <TRANSFORMATION> (0:01–0:04): The <object> <action verb — releases, lands, tips, opens>. From the <action point>, <transformation medium — black ink, light, color> does NOT spread as <banned trigger words: vines, roots, tentacles, organic growth>. Instead, the <medium> <PURPOSEFUL verb — draws itself, forms, resolves> into <a coherent named subject — a portrait, a comic illustration, a logo, a pattern>: <specific named details — heroic figure mid-leap, panel borders, speed lines, a starburst>. The lines are PURPOSEFUL strokes, not chaotic spreading. Camera slowly pulls back from the surface to reveal <the protagonist — use <PROTAGONIST> reference> holding the <object> on their <body part>. The walls dissolve from <previous setting> into <new setting>, with <lighting and atmosphere of new setting>.

BEAT 3 — <DIALOGUE BEAT> (0:04–0:06): Camera angle is now <over-the-shoulder mid-shot / close-up / etc.>, focused DIRECTLY ON THE <SUBJECT OF THE DIALOGUE — the artwork, the letter, the photo, the gift>. The <subject> fills the lower half of the frame. <SUPPORTING CHARACTER — use <SUPPORTING> reference> leans into frame from the <left/right>, their face partially in view, looking down at the SAME <subject>. Hold this composition — the <subject> is the focal point. <SUPPORTING CHARACTER>'s whispered voice says: "<verbatim line>". The line is delivered while the camera holds on <named visual>.

BEAT 4 — <REVEAL / CONFIRMATION> (0:06–0:07): One-beat pause after the line. <SUPPORTING CHARACTER>'s hand enters from the right holding <a confirming object — business card, key, document, photo>: "<text on the object, if any>". Held at a natural angle in <character>'s fingers — NOT macro close-up, mid-shot framing with shallow depth so the <object> is sharp and <character>'s <clothing> is soft behind. Subtle dolly-in for emphasis.

Audio: Soft solo piano holding a single note across Beat 1. Gentle <transformation sound — wet impact, soft whoosh, quiet click> on the Beat 2 transition. Subtle <texture sound — pen-on-paper, paper rustle, fabric brush> layered under the transformation. <Setting ambience — bus engine hum, traffic, café murmur> during Beats 3-4. <SUPPORTING CHARACTER>'s voice (whispered, warm, mid-40s female, slightly breathy with awe): "<verbatim line>". Single soft piano chord resolves over the Beat 4 reveal.

NEGATIVE PROMPTS: no giant <objects>, no oversized <object>, no <object> larger than 5mm, no ink vines, no ink tentacles, no ink roots, no organic growth patterns, no creepy spreading, no abstract splatter, no horror imagery.
```
````

---

## Quality checklist (run before saving)

For each clip, confirm all of the following:

- [ ] Style preamble names aspect, art direction, color palette
- [ ] Every beat has explicit `BEAT N — TITLE (0:00–0:0X):` header with timestamps
- [ ] Primary subject named in each beat; secondary subjects clarified as NOT the focus
- [ ] Real-world scale measurements (mm/cm/degrees/percent) wherever scale is ambiguous
- [ ] Camera framing class is named explicitly (wide, mid, ECU, OTS, low-angle, etc.)
- [ ] No metaphorical action verbs (no "vines/roots/tentacles/growth/spreading/blooming")
- [ ] Audio paragraph names per-beat sound design with timing
- [ ] Any dialogue is quoted verbatim with the on-screen visual named
- [ ] `NEGATIVE PROMPTS:` line lists clip-specific failure modes
- [ ] Reference attachments named: `**Attach references:** [IMG-X] + [IMG-Y]`
- [ ] Total clip duration is 4–7s (preferred); Seedance 2.0 allows up to 4–15s (`duration_min=4`, `duration_max=15`)
