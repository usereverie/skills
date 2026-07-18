# NodeFlow Reference (aggregate MCP)

The `nodeflow_*` tools operate the user's NodeFlow canvas through the same MCP
connection as the reverie tools. The graph you build is **visible and editable**
in the NodeFlow UI — the user can tweak any node by hand, so always re-read the
graph (`nodeflow_get_graph`) before you re-run, or you will clobber their manual
edits. `nodeflow://guide/*` resources are **NOT available** on this connection —
this file replaces them.

Build left → right by stage: **sources → processors → generators → sinks**.

## The loop
1. `nodeflow_describe_nodes` — node types, valid params, connection guidance.
2. `nodeflow_list_models(kind=image|video|text)` — valid `modelId` aliases with
   their sizes / ratios / resolutions / durations. Never invent an id.
3. `nodeflow_create_workspace` (or pick one from `nodeflow_list_workspaces`).
4. Build the **whole graph in one `nodeflow_apply_batch`** (add + connect ops
   together — fewer round-trips than `nodeflow_add_node` / `nodeflow_connect`
   one at a time). Fix mistakes with `nodeflow_update_node`,
   `nodeflow_disconnect`, `nodeflow_delete_node`.
5. `nodeflow_run_and_wait(workspace_id, timeout_s=300)` — use 600+ for video.
   Or `nodeflow_run` + poll `nodeflow_run_status`; fetch assets with
   `nodeflow_get_results`.
6. Report result URLs. **Iterate, never rebuild:** `nodeflow_update_node`, then
   re-run only the changed branch with `from_node_id`. Before every re-run,
   `nodeflow_get_graph` to pick up the user's manual canvas edits.

## `apply_batch` payload shapes

`nodeflow_apply_batch` ops take **camelCase** keys; the standalone tools take
**snake_case** for the same concepts. Mixing them is the most common failed call.

| Concept | Standalone tool | `apply_batch` op |
|---|---|---|
| Node type | `nodeflow_add_node(node_type=…)` | `"nodeType": …` |
| Position | top-level `x` / `y` | nested `"position": {"x": …, "y": …}` |
| Target handle | `nodeflow_connect(target_handle=…)` | `"targetHandle": …` |

**The error for this mistake points at the wrong thing.** Passing `node_type` inside a
batch op does not report a missing field — it reports:

```json
{"ok": false, "error": "invalid_params", "field": "nodeType", "given": "",
 "reason": "unknown node type ''", "valid_options": [...]}
```

`"given": ""` reads as "you passed an empty string", which sends you inspecting your
*value*. If you see `given: ""` on a field you know you set, check the **casing of the
key** first.

Worked example — one shot row (prompt + first frame → video generator → preview):

```json
{"ops": [
  {"kind": "add_node", "id": "t1", "nodeType": "textNode",
   "position": {"x": 0, "y": 0}, "params": {"text": "she types, then pauses"}},
  {"kind": "add_node", "id": "i1", "nodeType": "imageNode",
   "position": {"x": 0, "y": 110}, "params": {"imageUrl": "https://…"}},
  {"kind": "add_node", "id": "g1", "nodeType": "generatorNode",
   "position": {"x": 760, "y": 0},
   "params": {"type": "video", "modelId": "2.0 Pro-Fast", "ratio": "9:16",
              "resolution": "720p", "duration": 4, "generateAudio": false}},
  {"kind": "add_node", "id": "p1", "nodeType": "previewNode",
   "position": {"x": 1520, "y": 0}, "params": {}},

  {"kind": "connect", "source": "t1", "target": "g1"},
  {"kind": "connect", "source": "i1", "target": "g1", "targetHandle": "first_frame"},
  {"kind": "connect", "source": "g1", "target": "p1"}
]}
```

The `id` on an add op is a **batch-local alias** — connect ops in the same batch refer
to it directly, so you never need a round-trip to learn real node ids.

## Node types
11 types (from `nodeflow_describe_nodes`). Flow: `— → asset` reads left-to-right.

| type | role | key params | in → out |
|---|---|---|---|
| `textNode` | Prompt source | `text` | — → text |
| `imageNode` | Image source (account-hosted URL) | `imageUrl` | — → image |
| `generatorNode` | Generate image / video / edit | see below | text + images → asset |
| `previewNode` | Show a generator result | none | asset → — |
| `seedNode` | Prompt Enhancer — 1 prompt → 1 stronger prompt | `modelId`, `mode`, `temperature`, `maxTokens`, `thinking` | text → text |
| `promptGeneratorNode` | Text Iterator — 1 brief → N prompts | `modelId`, `count`, `temperature`, `maxTokens`, `thinking` | text → N texts |
| `anyLlmNode` | Any LLM call (instruction + context) | `modelId`, `prompt`, `temperature`, `maxTokens`, `topP`, `stop`, `thinking`, `responseFormat`, `jsonSchema`, `videoUrl`, `videoFps` | text(s) → text |
| `exportNode` | Save a result to gallery/storage | `prompt`, `modelId` | asset → gallery |
| `visualAnalysisNode` | Image Describer — image(s) → structured text | `modelId`, `forwardImages`, `temperature`, `maxTokens`, `thinking` | image(s) → text |
| `lastFrameNode` | Extract an upstream video's last frame as an image (shot chaining) | **none** | video generator → image |
| `stitchNode` | Join 2+ videos in `in-0..in-7` order, hard cuts | **none** | 2+ video generators → asset |

**`generatorNode` params** (image and video sets are mutually exclusive — set
the ones matching `type`):
- `type` — `image` \| `video` \| `edit`
- `modelId` — short alias from `nodeflow_list_models` (e.g. `4.5`, `2.0 Pro`)
- **image:** `size` (e.g. `2048x2048`), `aestheticMode`
  (`balanced` \| `high_aesthetic` \| `photorealism` \| `cinematic`),
  `guidanceScale` (supported models only), `outputFormat` (`jpeg` \| `png`),
  `batchSize` (1..model max, sequential)
- **video:** `ratio` (e.g. `9:16`), `resolution` (e.g. `720p`), `duration` (s,
  within model min/max), `generateAudio`, `serviceTier` (`default` \| `flex`),
  `frames` (`25+4n`, overrides duration; supported models only), `draft`
  (1.5 Pro only), `returnLastFrame`, `referenceVideoUrls`, `referenceAudioUrls`
- **both:** `seed`

`lastFrameNode` / `stitchNode` accept **only a video `generatorNode`** source —
gated server-side at connect time. A `lastFrameNode` takes exactly one incoming
edge; each of a `stitchNode`'s `in-0..in-7` handles must be free and every input
must match the others' ratio + resolution.

**Model quick-reference** (always confirm with `nodeflow_list_models` — this is a
convenience map, not the source of truth):
- Image `modelId`: `5.0-Pro` (max 1 sequential image), `5.0` / `4.5` / `4.0`
  (up to 10 sequential images), `5.0-Lite` (supports `guidanceScale`,
  `outputFormat`). "Sequential images" is `batchSize`/output count, not a
  reference-image cap — image models don't expose `maxRefImages`; for an image
  model's reference-image support, read its capabilities from `list_models`.
- Video `modelId`: `1.5 Pro` (480p–1080p, 4–12s, audio, `draft`),
  `1.0 Pro` / `1.0 Pro-Fast` (1–12s, **no audio**; `Pro-Fast` has **no last
  frame**; `maxRefImages` = 0), `2.0 Pro` (up to 4K, 4–15s, audio,
  `maxRefImages` = 9), `2.0 Pro-Fast` (480/720p, audio), `2.0 Mini`
  (480/720p, no audio). `maxRefImages` is a **video-model-only** field.
- For `first_frame` chaining, pick a video model with first/last-frame support
  (all except `1.0 Pro-Fast`, which cannot emit a last frame).

## Layout
Grid, one branch per row, so the canvas looks hand-arranged:
- `x = 0` — sources (`textNode`, `imageNode`)
- `x = 380` — processors (`seedNode`, `promptGeneratorNode`, `anyLlmNode`, `visualAnalysisNode`)
- `x = 760` — generators (`generatorNode`)
- `x ≈ 1000` — `lastFrameNode` (continuity extractor, just right of its source generator)
- `x ≈ 1200` — `stitchNode` (assembles the shot generators)
- `x ≈ 1520` — sinks (`previewNode`, `exportNode`) — right of the `stitchNode` they show
- `y += 220` per row; a branch's source / processor / generator / preview share one `y`.

In a chained sequence the `lastFrameNode → next generator's first_frame` edge often
runs leftward — that's fine; grid positions are cosmetic, not enforced.

Pass an explicit position on every add op — never stack at `(0,0)`. Note the two
shapes: inside a `nodeflow_apply_batch` add op use nested `position: {x, y}`; the
standalone `nodeflow_add_node` tool takes top-level `x` / `y`. Keep it to
**8–12 nodes**; beyond that, use a second workspace.

## Graph shapes
Distilled canonical shapes (all left → right):

1. **Simple image** — `textNode → generatorNode(type=image) → previewNode`.
2. **Enhanced pipeline** (default for a brief) —
   `textNode → seedNode → generatorNode(image) → previewNode → exportNode`.
3. **Variation fan-out** —
   `textNode → promptGeneratorNode(count=N) → N × generatorNode → previewNodes`.
4. **Reference-driven** —
   `imageNode → visualAnalysisNode → anyLlmNode → generatorNode(image) → previewNode`.
   Also wire `imageNode` straight into the generator when the image model
   accepts reference images — check that model's reference-image support via
   `list_models` (image models don't expose a `maxRefImages` field).
5. **Video from stills** —
   `textNode(motion) + imageNode(first frame) → generatorNode(type=video) → previewNode`.
   ⚠️ If the still shows a **human face**, this shape is filter-gated — a photorealistic
   face on the `first_frame` handle is rejected even when fully AI-generated. Settle the
   character source before building the graph (see *Human subjects in video* in
   `mcp-reference.md`).
6. **Free-form LLM step** — `anyLlmNode`: instruction in `prompt`, context from
   connected text (briefs → shot lists, style transfer, rewriting).

### Shot chaining + stitching
Multi-shot vertical video (e.g. TikTok) is why `lastFrameNode` and `stitchNode`
exist. One **row per shot**:

- **Shot:** `textNode(shot prompt) → generatorNode(video, ratio 9:16)`.
- **Continuity:** `generatorNode → lastFrameNode → next generator's` **`first_frame`**
  handle — the next shot opens on the previous shot's closing frame.
- **Assembly:** every shot generator → `stitchNode` (`in-0`, `in-1`, … = cut
  order) → `previewNode`. Max **8 shots per stitch** (handles `in-0..in-7`).
- **All `stitchNode` inputs must share ratio + resolution.** Hard cuts only —
  for a cut-only sequence, skip `lastFrameNode` and wire generators straight to
  `stitchNode`.

Edge rules are enforced at connect time: an invalid source or handle (e.g. a
non-video into `lastFrameNode`, an occupied `in-N`, mismatched dimensions) is
rejected with `invalid_params` naming the field — fix and retry, don't re-guess.

## Media inputs
- **Local file** → `nodeflow_upload_image` (base64, ≤10 MB).
- **User's past generations** → `nodeflow_search_gallery(query=...)`.
- **Web image** → `nodeflow_import_image_url` (cloned to the user's account).
  Never wire a raw external URL into an `imageNode` — its `imageUrl` must be
  account-hosted.
- **Canvas drops** by the user are already account-hosted — read them from
  `nodeflow_get_graph`.

## Errors
- **`invalid_params`** — the response names the field, the bad value, and
  `valid_options`. Fix using those options and retry; do not re-guess blindly.
  If it names a field you believe you set and reports `given: ""`, check the key's
  casing — see `apply_batch` payload shapes above.
- **`insufficient_credits`** — stop and tell the user; do not retry.
- **`partial`** from `nodeflow_run_and_wait` — some tasks are still running.
  Keep polling `nodeflow_run_status` with the returned `task_ids`.
- **`InputImageSensitiveContentDetected.PrivacyInformation`** on a node — a human
  likeness was rejected on video input. **Stop the whole run, don't retry the node.**
  Content-policy rejections escalate against the account; see *Safety gating &
  blocking errors* in `mcp-reference.md`. Fix the character source before re-running.
- **`Account is suspended. Generation is not permitted.`** on a node — an account-wide
  compliance gate, not a canvas problem. Every other node in the run will fail the same
  way and re-running fixes nothing. Report it and stop.

### Probe one node before running a batch

`nodeflow_run_and_wait` runs every node in the workspace — so when a run trips a gate
that escalates per attempt, one call can spend several rungs of the ladder before you
see the first error.

For any run whose inputs could be filter-sensitive — **human faces above all** — build
the full graph, then run **one row first** with `from_node_id`, confirm it succeeds, and
only then run the rest. The extra round-trip costs seconds; discovering the problem
three nodes deep can cost the account.

> Synced against backend 12f352d / nodeflow-mcp cf82821 on 2026-07-18.
