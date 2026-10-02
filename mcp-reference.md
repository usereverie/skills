# Reverie MCP Reference

Parameter tables, model capabilities, constraints, and tool documentation for the `reverie` MCP server.

Load this file when actually calling MCP tools — see `SKILL.md` in the same directory for the workflow router (direct vs plan mode), product naming, and folder terminology.

**Pre-flight:** before generating, you can call `list_models` to see what's available, `get_credit_balance` to check credits, and `get_generation_capacity` to check how many generations can run concurrently right now (plan `limit`, current `in_flight`, and free `available` slots — not a credit quota).

---

## Image Generation (`generate_image`)

Generate images from text prompts using Seedream models.

### Parameters

| Parameter | Type | Default | Options | Required |
|-----------|------|---------|---------|----------|
| `prompt` | string | — | Free text | Yes |
| `model` | string | `"4.0"` | `"5.0-Pro"`, `"5.0-Flash"`, `"5.0"`, `"5.0-Lite"`, `"4.5"`, `"4.0"` | No |
| `resolution` | string | `"1K"` | `"1K"`, `"1.5K"`, `"2K"`, `"3K"`, `"4K"` (per-model — see Model Capabilities) | No |
| `batch_size` | int | `1` | 1–10 | No |
| `aspect_ratio` | string | `"1:1"` | `"1:1"`, `"16:9"`, `"4:3"`, `"3:4"`, `"9:16"`, `"21:9"`, `"adaptive"` | No |
| `watermark` | bool | `false` | — | No |
| `optimize_prompt` | string | `"auto"` | `"auto"`, `"fast"`, `"off"` | No |
| `aesthetic_mode` | string | `"balanced"` | `"balanced"`, `"high_aesthetic"`, `"photorealism"`, `"cinematic"` | No |
| `reference_image_urls` | list[str] | `null` | Image URLs (third-party auto-cloned into account storage), up to 9 | No |
| `project_name` | string | `null` | Case-insensitive Studio project name (defaults to "General") | No |
| `name` | string | `null` | Friendly name for the output visual | No |
| `seed` | int | `null` | `-1` to `2147483647` for reproducibility | No |
| `output_format` | string | `null` | `"jpeg"`, `"png"` — models with `supports_output_format` only (`5.0-Lite`) | No |
| `guidance_scale` | float | `null` | 1–10 — models with `supports_guidance_scale` only (`5.0-Lite`) | No |
| `background` | string | `null` | `"transparent"` → PNG with alpha (`5.0-Pro` / `5.0-Flash`); needs exactly one reference image that has an alpha channel; `output_format` must be omitted or `"png"`. `"opaque"` = default | No |

> **Defaults are cost-optimised** (`4.0` @ `1K`, single image) — always pass `model` and `resolution` explicitly rather than relying on the default when the user wants higher quality. Optional parameters left `null` are not sent; passing `output_format` or `guidance_scale` to a model that doesn't support it returns an error.

### Model Capabilities

| Model | Best For | Resolution | Max sequential (batch) images | `guidance_scale` | `output_format` |
|-------|----------|-----------|--------------------------------|------------------|-----------------|
| **5.0-Pro** | Single highest-fidelity hero shot | 1K, 1.5K, 2K | 1 — don't pass batch params | No | default |
| **5.0-Flash** | Pro-quality single image at the lowest price ($0.018) | 1K, 1.5K, 2K | 1 — don't pass batch params | No | default |
| **5.0** | Top quality, up to 10 sequential | 2K, 3K, 4K | 10 | No | default |
| **4.5** (recommended) | High quality, up to 10 sequential | 2K, 3K, 4K | 10 | No | default |
| **4.0** | Legacy, widest resolution range | 1K, 2K, 3K, 4K | 10 | No | default |
| **5.0-Lite** | Fast; tunable guidance + format | 2K, 3K, 4K | 10 | Yes (1–10) | `"jpeg"`, `"png"` |

> "Max sequential (batch) images" is the `batch_size` cap (output count per call). The same knobs exist on the Flows canvas `generatorNode` as `guidanceScale` / `outputFormat`. Reference-image (`reference_image_urls`) support is a separate axis from batch size — see Constraints below. `list_models` is the live source for every flag in this table.

### Constraints
- Resolution is per-model: `5.0-Pro` / `5.0-Flash` = 1K/1.5K/2K (1.5K is priced as 1K); `5.0` / `4.5` / `5.0-Lite` = 2K/3K/4K; `4.0` = 1K/2K/3K/4K.
- `batch_size` > 1 is disabled at `1K` resolution.
- `batch_size` (sequential output images per call) max: **1 for `5.0-Pro` / `5.0-Flash`, up to 10 for `5.0` / `4.5` / `4.0` / `5.0-Lite`** — see Model Capabilities above.
- `reference_image_urls`: up to **9** per call on every image model (a platform cap below ModelArk's own limits).
- `guidance_scale` (1–10) and `output_format` (`jpeg` / `png`) are `5.0-Lite` only; other models reject them.
- `optimize_prompt="fast"` is honoured only where the model supports fast mode (`5.0-Pro`, `4.0` — `list_models` → `optimize_prompt_modes`); the others, **including `5.0-Flash`**, quietly run it as standard — no error.
- `background="transparent"`: `5.0-Pro` / `5.0-Flash` only, exactly one reference image with an alpha channel (a JPEG input is rejected by ModelArk), PNG output.
- `adaptive` aspect ratio fits the input reference image dimensions.

### Resolution Pixel Mapping (pixel-resolution image models)

These pixel dimensions apply to the pixel-resolution image models — `5.0-Pro`, `5.0`, `5.0-Lite`, `4.5`, and `4.0`. **Which resolutions are valid per model comes from the Model Capabilities table above** (e.g. `1K` only on `5.0-Pro` / `4.0`; `5.0` / `4.5` / `5.0-Lite` are 2K / 3K / 4K). Pick a resolution the chosen model supports, then read its pixel size below.

**1K:**
| Aspect Ratio | Pixels |
|-------------|--------|
| 1:1 | 1024x1024 |
| 16:9 | 1280x720 |
| 9:16 | 720x1280 |
| 4:3 | 1152x864 |
| 3:4 | 864x1152 |
| 21:9 | 1536x658 |

**2K (default):**
| Aspect Ratio | Pixels |
|-------------|--------|
| 1:1 | 2048x2048 |
| 16:9 | 2688x1512 |
| 9:16 | 1512x2688 |
| 4:3 | 2400x1800 |
| 3:4 | 1800x2400 |
| 21:9 | 3456x1480 |

**3K:**
| Aspect Ratio | Pixels |
|-------------|--------|
| 1:1 | 3072x3072 |
| 16:9 | 3200x1800 |
| 9:16 | 1800x3200 |
| 4:3 | 3072x2304 |
| 3:4 | 2304x3072 |
| 21:9 | 3584x1536 |

**4K:**
| Aspect Ratio | Pixels |
|-------------|--------|
| 1:1 | 3200x3200 |
| 16:9 | 4096x2304 |
| 9:16 | 2304x4096 |
| 4:3 | 3584x2688 |
| 3:4 | 2688x3584 |
| 21:9 | 4096x1792 |

---

## Image Variant (`create_variant`)

Regenerate an existing image visual with a new prompt, **replacing it in place**. Use this whenever the user is iterating on a visual they already generated — "try the same shot at sunset", "make it more dramatic", "regenerate, keep the style". The visual's row is updated; the previous prompt + asset are archived into the visual's variations history. Nothing is deleted, and no duplicate visual is created.

> **Use this instead of `generate_image` for any iterate / regenerate / "try again" request on an existing visual.** Calling `generate_image` for iteration creates a duplicate the user has to clean up manually — `create_variant` does not.

### Parameters

| Parameter | Type | Default | Notes | Required |
|-----------|------|---------|-------|----------|
| `visual_id` | string | — | ID of the existing image visual to regenerate in place | Yes |
| `prompt` | string | — | New prompt | Yes |
| `model` | string | parent's | Override with any image model (`"5.0-Pro"`, `"5.0"`, `"4.5"`, `"4.0"`, `"5.0-Lite"`); defaults to parent's | No |
| `resolution` | string | parent's | Valid values depend on the chosen model (see `generate_image` → Model Capabilities); defaults to parent's | No |
| `aspect_ratio` | string | parent's | Override `"1:1"`, `"16:9"`, etc. | No |
| `aesthetic_mode` | string | parent's | Override `"balanced"` / `"high_aesthetic"` / `"photorealism"` / `"cinematic"` | No |
| `optimize_prompt` | string | parent's | Override `"auto"` / `"fast"` / `"off"` | No |
| `watermark` | bool | parent's | Override watermark flag | No |
| `reference_image_urls` | list[str] | parent's | Override reference images | No |

Anything omitted is inherited from the parent visual. The variant stays in the same project (folder) as the parent.

### Constraints
- **Image visuals only.** Calling `create_variant` on a video visual returns an error. For video iteration, regenerate via `generate_video`.
- **batch_size is fixed at 1.** Each variant updates the single visual in place. To get multiple alternatives, call `create_variant` repeatedly — each call archives the previous state into the visual's variations history.

### When to use which image tool

| Tool | Use when |
|------|----------|
| `generate_image` | Brand-new visual from scratch (no existing visual to iterate on). |
| `edit_image` | Targeted instruction-based edit on an existing image (Seedream i2i) — e.g. "remove the person on the left", "change background to a beach". Produces a *new* visual; the original is preserved separately. |
| `create_variant` | The user is iterating the *prompt* of an existing visual ("same scene at sunset", "try it more cinematic"). Updates in place; no duplicate. |

### Example
```
generate_image(prompt="a quiet harbor at noon", model="4.5", resolution="2K", aspect_ratio="16:9", aesthetic_mode="cinematic")
# → returns visual id "v_abc123"

create_variant(visual_id="v_abc123", prompt="a quiet harbor at sunset")
# Same id "v_abc123", new asset. Old prompt + asset archived under metadata.variations[].

create_variant(visual_id="v_abc123", prompt="stormy harbor", aesthetic_mode="photorealism")
# Same id again. Override aesthetic_mode; everything else still inherited from the original (model 4.5, 2K, 16:9).
```

---

## Image Edit (`edit_image`)

Apply a targeted, instruction-based edit to an existing image (Seedream i2i mode) — e.g. "make the bubbles heart-shaped", "change the background to a beach", "remove the person on the left". Produces a **new** visual; the original is preserved separately. Synchronous — the edited asset URL is returned in the same call.

> When to reach for which image tool: see the comparison table in the `create_variant` section above. Rule of thumb — `generate_image` for new images from scratch, `edit_image` for targeted edits to a specific image, `create_variant` for re-prompting an existing visual in place.

### Parameters

| Parameter | Type | Default | Options | Required |
|-----------|------|---------|---------|----------|
| `prompt` | string | — | Description of the edit to apply | Yes |
| `image_url` | string | — | Public URL of the image to edit | Yes |
| `model` | string | `"4.5"` | `"4.5"`, `"5.0-Pro"`, `"5.0-Flash"`, `"5.0"`, `"5.0-Lite"`, `"4.0"` | No |
| `seed` | int | `null` | `-1` to `2147483647` for reproducibility | No |
| `watermark` | bool | `false` | — | No |
| `project_name` | string | `null` | Case-insensitive Studio project name (defaults to "General") | No |
| `name` | string | `null` | Friendly name for the output visual | No |
| `guidance_scale` | float | `null` (server default 5.5) | 1–10 — applied only by `5.0-Lite`; other models ignore it | No |
| `background` | string | `null` | `"transparent"` → PNG with alpha (`5.0-Pro` / `5.0-Flash`); `image_url` must itself have an alpha channel | No |

> `edit_image` has no `output_format` parameter — the edit endpoint does not take one.

### Constraints
- Input image must be JPEG or PNG, ≤ 10 MB, aspect ratio between 1:3 and 3:1, and ≤ 36,000,000 total pixels.
- `edit_image` always produces a *new* visual row — it does not modify the source. To iterate on the *prompt* of a visual in place, use `create_variant` instead.

---

## Video Generation (`generate_video` + `check_generation_status`)

Generate videos from text prompts using Seedance models. Video generation is **async** — you get a `task_id` back and must poll `check_generation_status` (the unified status tool — works for video task IDs and image visual IDs). Note the parameter name: `check_generation_status` takes `generation_id`, not `task_id` — pass the `task_id` value there.

> **Audio default:** Videos should be generated **with audio** by default. Pass `generate_audio=true` and use an audio-capable model (`"2.5"`, `"2.0 Pro"`, `"2.0 Pro-Fast"`) unless the user explicitly asks for a silent / no-audio video. This is especially important when the prompt contains speech cues like "make it say", "says", "speaks", "narrates", "sings", "voiceover", or references music, dialogue, or sound effects. Audio doubles generation cost (2x) — mention this once when the user's prompt is expensive or ambiguous, but don't ask permission for every clip.

### Parameters

| Parameter | Type | Default | Options | Required |
|-----------|------|---------|---------|----------|
| `prompt` | string | — | Free text | Yes |
| `model` | string | `"2.0 Mini"` | `"2.0 Mini"`, `"2.0 Pro"`, `"2.0 Pro-Fast"`, `"2.5"`, `"1.0 Pro"`, `"1.0 Pro-Fast"` | No |
| `duration` | int | `5` | Seconds — 1.0 family: 1–12; 2.0 family: 4–15; `"2.5"`: 4–30. `-1` = Smart duration (`"2.0 Pro"`, `"2.0 Pro-Fast"`, `"2.5"`) | No |
| `aspect_ratio` | string | `"16:9"` | `"16:9"`, `"4:3"`, `"1:1"`, `"3:4"`, `"9:16"`, `"21:9"`, `"adaptive"` (Smart ratio — 2.0 family + `"2.5"`) | No |
| `resolution` | string | `"480p"` | `"480p"`, `"720p"`, `"1080p"`, `"4K"` — `4K` is `"2.0 Pro"` only; `"2.0 Pro-Fast"` / `"2.0 Mini"` cap at `720p` | No |
| `first_frame_url` | string | `null` | Public image URL — literal opening frame | No |
| `last_frame_url` | string | `null` | Public image URL — literal closing frame | No |
| `reference_image_urls` | list[str] | `null` | Public image URLs — style/character refs (NOT literal frames) | No |
| `reference_video_urls` | list[str] | `null` | Reference videos — `"2.0 Pro"` / `"2.0 Pro-Fast"` up to 3, `"2.5"` up to 10. **Must be account-hosted** (`create_media_upload`) or `asset://` — not auto-cloned | No |
| `reference_audio_urls` | list[str] | `null` | Reference audio — same models, caps and hosting rule as `reference_video_urls` | No |
| `generate_audio` | bool | `null` → **pass `true` by default** (skill) | Audio-capable models only | No |
| `audio_sync` | bool | `false` | **Deprecated alias** of `generate_audio` — ignored when `generate_audio` is set | No |
| `camera_fixed` | bool | `null` | Lock the camera — 1.0 family only | No |
| `service_tier` | string | `null` | `"default"`, `"flex"` (half price, slower) — models with `supports_flex_tier` only (none today) | No |
| `seed` | int | `null` | `-1` to `4294967295` | No |
| `frames` | int | `null` | `25 + 4n` in `[29, 289]`, overrides `duration` — 1.0 family only | No |
| `watermark` | bool | `null` | Watermark the output | No |
| `return_last_frame` | bool | `null` (server default on) | Also return the watermark-free last frame, for chaining clips | No |
| `draft` | bool | `null` | Cheap 480p preview — `"2.5"` only (`supports_draft`); any `resolution` is coerced to 480p. Promote with `finalize_draft_video` (see Draft two-step) | No |
| `task_type` | string | `null` | `"2.5"` only — `"text_to_video"`, `"reference_to_video"`, `"edit"`, `"extend"`, `"first_last_frame"` (see below) | No |
| `input_mode` | string | `null` | `"frames"` or `"references"` — records the input mode; inferred when omitted | No |
| `project_name` | string | `null` | Case-insensitive Studio project name (defaults to "General") | No |
| `name` | string | `null` | Friendly name for the output visual | No |

> Optional parameters left `null` are not sent, so server defaults apply. The authoritative per-model constraints — valid resolutions, duration range, `adaptive` ratio, audio support, reference limits, and every `supports_*` flag — come from `list_models`. Validate model-specific choices against it before generating; unsupported combinations come back as an error.

### `task_type` (Seedance `"2.5"` only)

`"2.5"` reads its references as one of several intents. Say which with `task_type`; when you don't, it is inferred from the inputs and the prompt.

| `task_type` | Use when | Required inputs / locks |
|-------------|----------|-------------------------|
| `"text_to_video"` | No references | — |
| `"reference_to_video"` | New clip guided by reference images / videos / audio | — |
| `"edit"` | Change the visuals or audio of an existing video | ≥1 reference video (4–30 s); `aspect_ratio="adaptive"`; `duration=-1` |
| `"extend"` | Continue an existing video forward or backward | ≥1 reference video; `aspect_ratio="adaptive"` |
| `"first_last_frame"` | Clip between a given first and last frame | `aspect_ratio="adaptive"` |

`"edit"` and `"extend"` are also passed to ModelArk as a task-type hint, so their requirements (e.g. an edit with no reference video, or a reference video outside 4–30 s) are rejected **immediately** with `InvalidParameter.TaskTypeConstraint` instead of failing minutes later. The other intents send no hint — the model classifies from the inputs and prompt. The model still re-reads the prompt at run time: an `"edit"` whose prompt asks for a brand-new scene can still fail later with `InvalidParameter.TaskTypeMismatch`, so make the prompt match the intent ("Edit the video: …", "Extend the video: …").

For a locked intent, leave `aspect_ratio` / `duration` at their defaults (don't pass them) and the locked values are filled in for you; explicitly passing a conflicting value (e.g. `aspect_ratio="16:9"` with `task_type="edit"`) is an error.

### Frame mode vs Reference mode

Two mutually-exclusive ways to give the model an image:

- **Frame mode** (`first_frame_url` / `last_frame_url`): the image becomes the literal first/last frame of the video. Use when the user wants the generated clip to start/end on an exact picture they provided.
- **Reference mode** (`reference_image_urls`, plus `reference_video_urls` / `reference_audio_urls` on models that take them): the image is treated as a style/character reference. The model is guided by it but the image is *not* inserted as a frame. Use when the user says "use this as a character reference", "in the style of", "match this look", etc.

**Per request, never combine the two** — the backend rejects mixed payloads. Pick one mode based on the user's intent.

| Mode | Models that support it | Max images |
|------|------------------------|-----------|
| Frame (first only) | All Seedance models | 1 |
| Frame (first + last) | 1.0 Pro, 2.0 Pro, 2.0 Pro-Fast, 2.0 Mini, 2.5 (NOT 1.0 Pro-Fast) | 2 |
| Reference | 2.0 Pro, 2.0 Pro-Fast, 2.0 Mini, 2.5 | 2.0 Pro / 2.0 Pro-Fast: 9 images + 3 videos + 3 audio; 2.0 Mini: 9 images only; 2.5: 30 images + 10 videos + 10 audio (audio-only allowed) |

### Human subjects in video — ModelArk digital-asset gating

ModelArk gates recognisable human likeness on **video input**. A photorealistic human
face reaching a video model as `first_frame_url` is rejected upstream with:

```
InputImageSensitiveContentDetected.PrivacyInformation
"The request failed because the input image may contain real person."
```

**This fires on fully synthetic faces.** A Seedream character sheet generated with
`aesthetic_mode="photorealism"` trips it. The filter tests likeness, not provenance — it
has no way to know the face was never real, and the better your character-consistency
chain works, the more reliably it trips. Image generation is not gated this way; only
video ingestion is. So a chain can pass Phase A cleanly and fail 100% of Phase B.

`aesthetic_mode` is an **image** parameter and has no effect on this **video** filter.
Dropping to `cinematic` or `balanced` is not a fix.

ModelArk accepts two compliant sources for a human character in video:

| Path | How | Notes |
|---|---|---|
| **Preset digital-character library** | Pass `asset://asset-…` as a reference image | ~5,900 pre-cleared portraits. No extra subscription. |
| **Actors / authorized real person** | Requires the authorized-real-person subscription on the account | For a specific real person with consent on file. |

`asset://` URIs reach ModelArk **verbatim** and are resolved server-side — never rewrite
one to `https://`. Because they are *reference* images, they cannot be combined with
`first_frame_url` in the same call (see Frame vs Reference above).

### Browsing the preset portraits — `list_digital_characters`

`list_digital_characters` returns 3 candidates at a time: a numbered text index
followed by one image each, in the same order. Show them to the user and let them
pick a number.

```
list_digital_characters(country="Malaysia", gender="female", age_min=28, age_max=42)
# → text index + 3 images; ask for offset=3 to see the next 3
generate_video(prompt="...", reference_image_urls=["asset://asset-…"], model="2.0 Pro")
```

Parameters: `q`, `gender`, `country`, `occupation`, `age_min`, `age_max`, `offset`.
There is no `limit` — the page size is fixed at 3 to keep image cost predictable.

**Use `q` for role-ish concepts.** `occupation` (93 terms) and `country` (95) are
closed vocabularies — invent a value like `"Bakery Owner"` and you get zero rows,
which reads as an empty catalog. `q="bakery"` searches free text including each
persona's bio and finds them anyway. On a zero-result structured query the tool
returns the valid values, so recover by retrying rather than telling the user the
catalog has nobody suitable.

**Do not rank the faces yourself.** The catalog stores no physical description, so
there is nothing to rank on — present the candidates and let the user choose.

**Plan the character source at spec time, before spending stills credits.** For a
human-subject video job, pick one:

- **Faceless framing** — hands, over-the-shoulder, from behind, or face out of frame.
  The still → `first_frame_url` chain works normally. Cheapest fix; often no creative loss.
- **Preset portrait** — build on an `asset://` portrait in reference mode from the start.
  Trades literal frame control for a character that clears the filter.
- **A face you generated** — fine for **stills**, but do not plan video off it.

### Private real-human assets — verified people (quota-gated)

For users granted a private-asset quota, five tools cover a user's own verified
people (each group = one liveness-verified real person; assets are face-matched
against that person and yield `asset://` URIs that pass the likeness filter):

| Tool | Purpose |
|---|---|
| `start_person_verification(person_name)` | Start identity verification. Returns an H5 link — **hand it to the human**: the person opens it on their phone and completes the face liveness scan (single-use, 30-min expiry). The group is created under `person_name`. |
| `check_verification_status(session_id)` | Poll ~every 30 s until `completed` (returns the new `group_id`), or `failed`/`expired` (start a new session). |
| `upload_private_asset(group_id, url, name?)` | Add an asset by public https URL (image ≤30MB / video ≤50MB / audio ≤15MB). Ingestion is async and face-matched — starts as `Processing`. |
| `check_private_asset_status(asset_id)` | Re-sync from ARK. `Active` → use `asset://<id>` as a generation reference; `Failed` → error code/message (face mismatch is the common cause). |
| `list_private_assets(group_id?, asset_type?, offset?)` | Browse Active assets, 3 per page with thumbnails. |

The liveness step is the one thing an agent cannot do — always relay the H5
link and wait. Renaming/deleting people or assets is **not** available via
MCP: direct the user to **Sources → Private people** in the Studio UI (click a
group's name to rename it; trash icon "Delete group" removes it and all its
assets; per-asset pencil/trash icons rename/delete one asset).

Quota errors (403/409) mean the account has no private-asset quota (admin-set)
or all person slots are used.

### Audio / speech behaviour

- **Default to `generate_audio=true`** for every video request, unless the user says "no audio", "silent", "mute", "no sound", or similar. Use any audio-capable model: `2.5`, `2.0 Pro`, or `2.0 Pro-Fast`.
- If the user's prompt asks the subject to say, sing, narrate, or speak specific words, **include the exact target line inside the `prompt`** (e.g. `...the presenter says: "Assalamualaikum, hari ini Re:source Friday saya ke 10"`). Keep the quoted line verbatim — do not translate or paraphrase.
- If the user picks (or you must pick) a model that does **not** support audio (`1.0 Pro`, `1.0 Pro-Fast`, `2.0 Mini`), warn them that the resulting video will be silent and offer `2.5`, `2.0 Pro`, or `2.0 Pro-Fast` as the audio-capable alternative before generating.
- Never silently drop the audio parameter. If you override the default (e.g. to save cost), say so in your response.

### Model Capabilities

| Model | Quality | Resolution | First / Last Frame | Reference Images | Audio Sync | Smart Ratio | Duration | Notes |
|-------|---------|-----------|--------------------|------------------|------------|-------------|----------|-------|
| **2.5** | Newest | 480p / 720p / 1080p | Yes / Yes | **Yes (30 img + 10 vid + 10 aud)** | Yes (2x cost) | Yes | 4–30s | Longest clips; video edit / extend via `task_type`; audio-only refs; Smart duration |
| **1.0 Pro** | High | 480p / 720p / 1080p | Yes / Yes | No | No | No | 1–12s | Frame-only |
| **1.0 Pro-Fast** | Good | 480p / 720p / 1080p | Yes / **No** | No | No | No | 1–12s | Fastest, first-frame only |
| **2.0 Pro** | Premium | 480p / 720p / 1080p / **4K** | Yes / Yes | **Yes (9 img + 3 vid + 3 aud)** | Yes (2x cost) | Yes | 4–15s | Only model with **4K**; multimodal refs; Smart duration |
| **2.0 Pro-Fast** | Premium-fast | 480p / 720p | Yes / Yes | **Yes (9 img + 3 vid + 3 aud)** | Yes (2x cost) | Yes | 4–15s | Faster 2.0; caps at 720p; Smart duration |
| **2.0 Mini** | Premium-lite | 480p / 720p | Yes / Yes | **Yes (9 images only)** | **No** | Yes | 4–15s | Cheapest 2.0; **no audio**; **no Smart duration** (explicit `duration` required); caps at 720p |

### Constraints
- `4K` resolution is available on **`2.0 Pro` only**. `2.0 Pro-Fast` and `2.0 Mini` cap at `720p`; the 1.0 family and `2.5` top out at `1080p`.
- Image inputs (frame OR reference) do **not** restrict resolution — valid resolutions are per-model only. Check `list_models` before picking a combination.
- `last_frame_url` is NOT supported by `1.0 Pro-Fast` (first-frame only).
- `generate_audio` is supported by `2.5`, `2.0 Pro`, and `2.0 Pro-Fast` only; doubles the generation cost. `1.0 Pro`, `1.0 Pro-Fast`, and `2.0 Mini` are silent.
- **Duration:** 1.0 family (`1.0 Pro` / `1.0 Pro-Fast`) = 1–12s; 2.0 family (`2.0 Pro` / `2.0 Pro-Fast` / `2.0 Mini`) = 4–15s; `2.5` = 4–30s.
- **Smart ratio** (`adaptive` aspect ratio) is available on the whole 2.0 family and `2.5`; the 1.0 family is fixed-ratio only.
- **Smart duration** (`duration=-1`) is available on `2.0 Pro`, `2.0 Pro-Fast`, and `2.5`; `2.0 Mini` and the 1.0 family require an explicit `duration`.
- **References:** `2.0 Pro` / `2.0 Pro-Fast` accept up to **9 images + 3 videos + 3 audio**; `2.0 Mini` up to **9 images only**; `2.5` up to **30 images + 10 videos + 10 audio**, and audio may be the only reference. The 2.0 family needs an image or video alongside any audio. Video/audio reference URLs must be account-hosted (`create_media_upload` → `finalize_media_upload`) or `asset://`.
- `frames` and `camera_fixed` are 1.0-family only; the 2.0 family and `2.5` reject them.

### Draft two-step (`"2.5"` only) — `finalize_draft_video`

Preview a clip cheaply before paying for 1080p:

1. `generate_video(model="2.5", draft=True, prompt=..., ...)` — always renders at **480p**
   (any `resolution` you pass is coerced, not rejected). Poll `check_generation_status`
   until it succeeds and check the preview with the user.
2. `finalize_draft_video(task_id="<draft task_id>")` — creates the **1080p final** from the
   draft. Returns the final's `task_id` (poll it like any video), `visual_id`, and
   `estimated_credits`. The final appears next to the draft in the same visual thread.

| Parameter | Type | Default | Notes | Required |
|-----------|------|---------|-------|----------|
| `task_id` | string | — | The **draft** task id from step 1 | Yes |
| `watermark` | bool | `null` | Watermark the final | No |
| `return_last_frame` | bool | `null` (on) | Also return the final's last frame | No |
| `name` | string | `null` | Name for the final (defaults to the draft's) | No |

Rules (each failure returns an error naming the rule):
- The final **reuses** the draft's prompt, references, ratio, duration, seed and audio
  setting — to change any of them, make a new draft.
- Only a **succeeded** draft can be finalized; a draft can be finalized **once** (retry is
  allowed only if the previous final failed).
- Draft ids expire **7 days** after creation (finalize is refused from 6 days 23 hours).
- Billing: the draft and the final are separate charges. The final is billed at the
  1080p rate — the lower *with-video-input* rate if the draft had reference videos.

### Workflow Pattern

```
1. Call generate_video(prompt="...", ...) → get task_id
2. Wait ~30-60 seconds
3. Call check_generation_status(generation_id=task_id) → check status
4. If status is "pending" or "processing", wait and poll again
5. If status is "succeeded", the response includes video_url
6. If status is "failed", the response includes error details
```

---

## 3D Generation (`generate_3d` + `check_3d_status`)

Generate 3D models from text and/or images using ModelArk's Hyper3D Gen2 or Hitem3d 2.0. **Async** — `generate_3d` returns a `task_id`; poll `check_3d_status` until `status` is `"succeeded"` or `"failed"`. Typical wall-clock is 2–8 minutes per model. ModelArk retains `file_url` for **7 days only** — download promptly.

### Model picker

| Model | Inputs | Outputs | Param surface | Cost |
|-------|--------|---------|---------------|------|
| **`hyper3d`** (Hyper3D Gen2, default) | Text + 1–5 images (jpg/jpeg/png, ≤ 30 MB, ≤ 4096×4096) | glb (default) / obj / usdz / fbx / stl | Rich — material, mesh mode, polycount, HD texture, T/A-pose, etc. | Fixed $0.399 / model |
| **`hitem3d`** (Hitem3d 2.0) | 1–4 images only (jpg/jpeg/png/webp, ≤ 10 MB) — no text semantics | obj / glb / stl / fbx / usdz (passed as integer 1–5) | Narrower — resolution, face count, request type, view bitmap | $0.80 – $1.80 / model |

### `generate_3d` parameters

Shared:

| Parameter | Type | Default | Notes | Required |
|-----------|------|---------|-------|----------|
| `model` | string | `"hyper3d"` | `"hyper3d"` or `"hitem3d"` | No |
| `prompt` | string | `null` | Hyper3D: English, ≤ 400 chars. Hitem3d ignores prompt semantics. | No |
| `image_urls` | list[str] | `null` | Public HTTPS URLs (Hyper3D also accepts base64 `data:` URIs). Order matters for Hitem3d when `multi_images_bit` is set — `[front, back, left, right]`. | No |
| `fileformat` | string | model-default | Hyper3D: `"glb"`/`"obj"`/`"usdz"`/`"fbx"`/`"stl"`. Hitem3d: integer 1–5 as string (`"1"`=obj, `"2"`=glb, `"3"`=stl, `"4"`=fbx, `"5"`=usdz). | No |
| `seed` | int | `null` | 0–65535 (same seed ≠ identical output). | No |
| `project_name` | string | `null` | Case-insensitive Studio project (defaults to "General"). | No |
| `name` | string | `null` | Friendly name for the task. | No |

Hyper3D-only:

| Parameter | Type | Default | Notes |
|-----------|------|---------|-------|
| `material` | string | `"PBR"` | `"PBR"` / `"Shaded"` / `"All"` / `"None"` |
| `mesh_mode` | string | `"Quad"` | `"Raw"` (triangle) or `"Quad"` |
| `quality_override` | int | Raw 500k / Quad 18k | Raw: 500–1,000,000. Quad: 1,000–200,000. Recommended ≥ 150,000. |
| `hd_texture` | bool | `false` | Enables 4K textures (forces HighPack). |
| `use_original_alpha` | bool | `false` | Preserves input transparency. |
| `addons` | string | `null` | `"HighPack"` for 4K texture maps (otherwise 2K). |
| `bbox_condition` | list[int] | `null` | `[x, y, z]` bounding-box override. Usually omit. |
| `ta_pose` | bool | `false` | Forces T/A-pose for humanoid models. |
| `subdivisionlevel` | string | `null` | `"high"` / `"medium"` / `"low"`. Ignored if `quality_override` is set. |

Hitem3d-only:

| Parameter | Type | Default | Notes |
|-----------|------|---------|-------|
| `resolution` | string | `"1536"` | `"1536"` or `"1536pro"` |
| `face` | int | `null` | Polygon count, 100,000 – 2,000,000 |
| `request_type` | int | `3` | `1` = geometry only, `3` = geometry + textures |
| `multi_images_bit` | string | `null` | 4-char bitmap for the views provided in `image_urls`, order `[front, back, left, right]`. Example: `"1010"` = front + left views. |

### `check_3d_status` parameters

| Parameter | Type | Notes | Required |
|-----------|------|-------|----------|
| `task_id` | string | The `task_id` returned by `generate_3d` | Yes |

Polling pattern: every ~15 seconds until `status` is `"succeeded"` or `"failed"`. On success the response contains `file_url`, `file_format`, and `completion_tokens`. On failure it contains `error_code` + `error_message`.

### Workflow

```
1. generate_3d(model="hyper3d", prompt="a stylized low-poly fox", fileformat="glb")
   # → { task_id: "...", model_id: "...", status: "queued" }
2. Wait ~30s, then poll check_3d_status(task_id) every ~15s.
3. When status == "succeeded": download file_url within 7 days.
```

> **3D models are not pollable via `check_generation_status`** — use `check_3d_status`. Conversely, video task IDs / image visual IDs are not pollable via `check_3d_status`. Different backing tables.

---

## Text Generation (`generate_text`)

Generate text using Seed LLM models.

### Parameters

| Parameter | Type | Default | Options | Required |
|-----------|------|---------|---------|----------|
| `prompt` | string | — | Free text | Yes |
| `model` | string | `"2.0 Pro LLM"` | `"2.0 Pro LLM"`, `"2.0 Lite"`, `"2.0 Mini"`, `"1.8"`, `"1.6"`, `"1.6-flash"` | No |
| `system_prompt` | string | `null` | Free text | No |

### Models
- **2.0 Pro LLM** — Latest and most capable.
- **2.0 Lite** — Balanced quality and cost.
- **2.0 Mini** — Fast, low cost.
- **1.8** — Strong general-purpose.
- **1.6** — Stable, good quality.
- **1.6-flash** — Fastest, lowest cost.

> These are Seed **LLM** models — the `2.0 Lite` / `2.0 Mini` here are text models, distinct from the Seedance **video** models of the same names.

---

## Safety gating & blocking errors

### Content-policy rejections are cumulative — stop the batch

A rejection carrying a content-policy code (e.g.
`InputImageSensitiveContentDetected.PrivacyInformation`) is not just a failed
generation. It is recorded as a **safety event against the account**, and safety events
escalate:

| Safety events | Window | Result |
|---|---|---|
| 1st | 30 days | warning — no status change, nothing visible to the user |
| 2nd | 30 days | account **suspended** |
| 3rd | 90 days | account **terminated** |

Because the first event is silent, the second one arrives as a surprise — and it locks
**all** generation, not just the call that tripped it.

**When a generation fails with a content-policy code: stop.** Do not retry it, and do
not move on to the next item in the batch. Report the flag to the user and let them
decide. Retrying a filter-tripping input is the single fastest way to escalate a
recoverable warning into a locked account, and a batch is the worst place to find out.

This is why human-subject video wants a **one-clip probe before the batch** — see
*Human subjects in video* above, and the canvas note in `flow-reference.md`.

### `Account is suspended. Generation is not permitted.` (403)

Reverie's **own compliance gate**, not a ModelArk error and not a billing problem. It
reads the account's status and blocks every generation tool — images, video, canvas,
3D, all of it — regardless of credit balance. Credits are untouched; nothing bills.

Diagnostic tell: a trivial unrelated `generate_image` also returns it. That confirms the
block is account-wide rather than specific to the input that tripped the filter.

It cannot be cleared from the MCP tools — it needs an operator status reset or an
appeal. **Surface it to the user plainly and stop.** Do not retry, do not try another
model, and do not switch to the canvas hoping for a different path; they all read the
same gate.

---

## Utility Tools

### Media inputs (images for generation / understanding)

ProjectReverie Visualfeed can only proxy known storage hosts. **Do not hotlink** product-page / Shopify / arbitrary CDN URLs into visuals.

| Source | Tool |
|--------|------|
| Local file on the agent machine | `create_media_upload(content_type, size_bytes, filename?)` → PUT bytes to `upload_url` → `finalize_media_upload(upload_id)` → use returned `asset_url` |
| Web / product-page image URL | `import_image_url(url)` → use returned `url` (account-hosted clone) |
| Already account-hosted / `asset://…` | Pass through to `generate_*` / `edit_image` |

MCP tools that accept image URLs (`edit_image`, `generate_image` refs, `generate_video` frames/refs) **auto-clone** third-party `http(s)` URLs via the same path before calling the API — but prefer calling `import_image_url` explicitly when the user pastes a web image.

### `import_image_url(url)`

Clone a public web image into the user's account storage (≤10 MB; PNG/JPEG/WebP/GIF). Returns `{"url": "<account-hosted>"}`. Errors: `unreachable_url`, `unsupported_media`.

### `create_media_upload` / `finalize_media_upload`

Direct upload of a local file (image ≤10 MB; video ≤50 MB; audio ≤15 MB). See tool descriptions for the 3-step flow. `finalize_media_upload` is the only source of the validated `asset_url`.

### `list_models`
Returns all available models with their IDs, display names, and pricing. Call this first to see what's available.

### `get_credit_balance`
Returns the current user's available credits along with `plan_slug` and `plan_credits_per_month`. Credits are the sole currency that gates generation — USD wallet balance is not exposed. Check before batch operations.

### `get_generation_capacity`
No parameters. Returns the account's **concurrency** state, not a credit quota: `limit` (the plan's concurrent-generation ceiling), `in_flight` (generations currently pending/processing), and `available` (free slots right now). Pairs with `get_credit_balance` (which covers spend/credits) for pre-flight checks before a batch.

> **`limit: 1` on a cold account is a fallback, not your real ceiling.** `limit` reads the
> plan's `max_concurrent_generations`, and falls back to **1** when the plan can't be
> resolved — which is exactly what happens before the account's first generation, since
> the user row is created lazily on that first call. A pre-flight capacity check on a
> fresh account therefore under-reports, often badly (a plan whose real ceiling is 8 will
> read as 1). Re-check after the first successful generation before committing to a
> strictly sequential execution shape, or you will serialize a batch that could have
> fanned out.

---

## Project Tools

Projects group generated images and videos under the "Recent Projects" section of the Studio UI. When the user asks to "create a folder", "put this in a project", or "start a new workspace" in a Reverie context, use these tools — never a filesystem folder. (See `SKILL.md` for the full folder/Project terminology rules.)

### `list_projects`
Returns the current user's projects (`id`, `name`, `slug`, `visual_count`). Call this to show the user where their generations can be saved, or to resolve a fuzzy project name before generating.

### `create_project(name)`
Creates a new project. Names must be unique per user (1–100 chars). After creating, pass the same `name` as `project_name` to `generate_image` / `generate_video` to save generations into it.

### Using projects with generation
Both `generate_image` and `generate_video` accept an optional `project_name` argument (case-insensitive). Omit it to save to the default `"General"` project.

```
create_project(name="reverie-linkedin-mei")
generate_image(prompt="...", project_name="reverie-linkedin-mei")
```

---

## Visual Lookup (`list_visuals`)

Find existing visuals the user wants to act on — typically before calling `create_variant` (which needs a `visual_id`) or when the user references something they generated earlier ("the harbor one", "clip-d-head-on-wheel").

### Parameters

| Parameter | Type | Default | Notes | Required |
|-----------|------|---------|-------|----------|
| `project_name` | string | — | Scope to one project (case-insensitive). Omit to search across all the user's projects. | No |
| `type` | string | — | `"image"`, `"video"`, or `"llm"`. Omit for all types. | No |
| `search` | string | — | Substring match (case-insensitive) against the visual's **prompt, name, and slug**. | No |
| `limit` | int | `20` | 1–100, most recent first. | No |

### Behaviour
- `search` matches `Visual.prompt`, `Visual.name`, **and** `Visual.slug` (kebab-case). So passing a slug verbatim — e.g. `search="clip-d-head-on-wheel"` — finds the visual directly. Partial slugs work too (`search="clip-d"`).
- Each item returned is trimmed to the agent-essential fields: `id`, `name`, `slug`, `type`, `prompt`, `model`, `asset_url`, `timestamp`, `folder_id`. Use the `id` for follow-up calls (e.g. `create_variant`).
- Top-level `total` indicates the unpaginated match count; raise `limit` (max 100) if the user asks for more.

### Typical chains

**Find then re-roll a specific visual the user names:**
```
list_visuals(search="clip-d-head-on-wheel")            # → [{ id: "v_abc...", ... }]
create_variant(visual_id="v_abc...", prompt="...")
```

**Enumerate a project before showing the user choices:**
```
list_visuals(project_name="reverie-linkedin-mei", type="image", limit=20)
```

**Find recent videos across all projects:**
```
list_visuals(type="video", limit=10)
```

> Use `list_visuals` instead of asking the user for an ID whenever they reference a visual by name, slug, or a description that maps to a prompt substring.

---

## Common Workflows

### Generate an image, then animate it as video
1. `generate_image(prompt="a serene mountain lake at sunset", model="4.5", aspect_ratio="16:9")`
2. Get the `asset_url` from the result
3. `generate_video(prompt="gentle water ripples and clouds drifting", first_frame_url="{asset_url}", model="2.5", generate_audio=true)`
4. Poll `check_generation_status(generation_id)` until complete

### Generate a video using a character/style reference (not a literal first frame)
1. Have one or more reference images (e.g. character sheets, mood boards) as public URLs.
2. `generate_video(prompt="the character walks through a neon-lit alley, cinematic", reference_image_urls=["{url1}", "{url2}"], model="2.0 Pro", generate_audio=true)` — use `2.0 Pro` (or `2.0 Pro-Fast`) for premium quality (up to 9 image refs), `2.5` for longer clips or more references (up to 30 image refs), or `2.0 Mini` for cheaper runs (up to 9 image refs, but no audio).
3. Poll `check_generation_status(generation_id)` until complete. The model takes inspiration from the references but doesn't insert them as literal frames.

### Animate an image with spoken line ("make it say …")
1. `generate_video(prompt='the subject says: "Assalamualaikum, hari ini Re:source Friday saya ke 10", natural lip-sync, keep framing consistent with the reference image', first_frame_url="{asset_url}", model="2.5", generate_audio=true)`
2. Poll `check_generation_status(generation_id)` until complete — the returned video will include the spoken audio.

### Iterate on a visual (regenerate with a different prompt)
1. `generate_image(prompt="cyberpunk cityscape", aesthetic_mode="cinematic")` → returns visual `v_abc`
2. User asks "try a daytime version" → `create_variant(visual_id="v_abc", prompt="cyberpunk cityscape at noon, bright")`
3. User asks "more rainy" → `create_variant(visual_id="v_abc", prompt="cyberpunk cityscape at noon, heavy rain")`

The visual stays as a single tile; previous prompts + assets are kept in `metadata.variations[]` so the user can scroll back. Do **not** call `generate_image` again for these iterations — it creates duplicates.

### Iterate on a visual from a previous session (user names it by slug)
The user says "regenerate clip-d-head-on-wheel with a different angle" — but the visual was generated before this conversation started, so there's no id in context.

1. `list_visuals(search="clip-d-head-on-wheel")` → returns `[{ id: "v_xyz...", ... }]`
2. `create_variant(visual_id="v_xyz...", prompt="...same subject, low-angle shot")`

### Batch generation with different styles (when the user wants alternatives side-by-side)
Use this only when the user explicitly wants multiple visuals to compare in parallel — not for iteration:
1. `generate_image(prompt="cyberpunk cityscape", aesthetic_mode="cinematic")`
2. `generate_image(prompt="cyberpunk cityscape", aesthetic_mode="photorealism")`
3. `generate_image(prompt="cyberpunk cityscape", aesthetic_mode="high_aesthetic")`

---

## Tips

- **Prompt quality matters.** Be specific about composition, lighting, style, and subject. The `optimize_prompt` setting can help improve vague prompts.
- **Check your balance** with `get_credit_balance` before batch operations.
- **Use 4.5 or 5.0 for images** (up to 4K, up to 10 sequential images); reach for `5.0-Pro` for a single highest-fidelity hero shot (1K/2K only, `batch_size` fixed at 1), or `5.0-Lite` for faster generation or `guidance_scale` / `output_format` (`jpeg`–`png`) tuning.
- **Picking a video model**: reach for `2.5` for audio + frame work, clips longer than 15 s, or editing / extending an existing video (`task_type`); `2.0 Pro` (or the cheaper `2.0 Pro-Fast`) when the user wants premium quality or 4K (2.0 Pro only). Use `2.0 Mini` (the tool default; up to 9 image refs, no audio) when cost matters more than premium quality. Default `generate_audio=true` unless the user asks for a silent clip — "make it say", "sings", and similar prompts imply audio is required.
- **Frame vs reference**: if the user wants the video to literally start (or end) on a specific image, use `first_frame_url` / `last_frame_url`. If they want the model to take stylistic/character cues from images without locking them as frames, use `reference_image_urls` (2.0 Pro, 2.0 Pro-Fast, 2.0 Mini, 2.5 only). Never combine both in the same call.
- **Video is async.** Always poll `check_generation_status` — do not assume instant results.
- **Media inputs:** local file → `create_media_upload` / `finalize_media_upload`; web image → `import_image_url` (or rely on auto-clone). Never depend on hotlinked third-party CDNs for Visualfeed display.

---

> Direct tools (`generate_image` / `edit_image` / `generate_video`) synced against backend
> `feat/mcp-parity-backfill` on 2026-10-01 and kept in step by `tests/test_mcp_parity.py`.
> Synced against backend 6819202 on 2026-08-13. (The flow tool surface was first
> synced from the standalone `nodeflow-mcp` repo at cf82821 on 2026-07-18 — that
> was the repo's name then; it is Flows now, but the provenance stands.)
