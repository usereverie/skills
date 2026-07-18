---
name: content-pipeline
description: Six-stage controlled content production — brainstorm, spec, plan, execute (NodeFlow canvas or direct tools), review/resolve, report. Files-first artifacts with chat fallback. Use for campaign/multi-asset/production content work, or when a /reverie:* stage command is invoked.
metadata:
  author: project-reverie
  version: "0.3.0"
---

# Content Production Pipeline

Six gated stages. NEVER skip a gate: each stage ends with explicit user
approval before the next begins. If the user invokes a stage command out of
order, check the prior stage's artifact exists first; if it doesn't, offer to
run the missing stage instead.

**Artifacts are files-first:** when you have a filesystem, write
`specs/content/YYYY-MM-DD-<slug>.md` and `plans/content/YYYY-MM-DD-<slug>.md`
in the working directory (create dirs as needed) and commit if the directory
is a git repo. **No filesystem (phone/claude.ai):** present the same content
as structured chat messages and get the same explicit approvals; the canvas
still holds the graph.

**Tool references:** read `../../mcp-reference.md` before generating;
read `../../nodeflow-reference.md` before any `nodeflow_*` call.

## Stage 1: Brainstorm
Refine the brief conversationally — ONE question at a time (audience, message,
platform/format, deliverable count, references, tone). Prefer multiple choice.
Stop when you can state the brief in <10 lines. GATE: user confirms the brief.

## Stage 2: Spec
Write `specs/content/YYYY-MM-DD-<slug>.md`:
- Audience, core message, tone.
- Deliverables table: name | type (image/video) | format (ratio, resolution, duration) | model.
- References (URLs / gallery items).
- Budget check: call `get_credit_balance` (remaining credits) + `get_generation_capacity`
  (in-flight / free slots). Give a qualitative fit assessment — "N deliverables of this
  kind vs your balance of X". Exact per-deliverable credit cost is often NOT derivable
  from the tools (video is billed per-second, not per-unit), so don't invent a
  false-precise number; flag if the batch looks close to the balance.
- Feasibility check — **does any video deliverable show a human face?** If so, the
  still → `first_frame_url` pattern is filter-gated (ModelArk rejects recognisable
  human likeness on video input, including faces you generated yourself). Settle the
  character source here, in the spec, with the user: faceless framing / preset
  `asset://` portrait in reference mode (browse via `list_digital_characters` and let
  the user pick) / stills-only. See *Human subjects in video* in
  `../../mcp-reference.md`. Getting this wrong is not a retry — the image phase bills
  and succeeds, and every video derived from it fails.
- Note the capacity caveat: a pre-flight `get_generation_capacity` on an account that
  hasn't generated yet reports the fallback `limit: 1`, not the plan's real ceiling.
  Don't harden a strictly-sequential execution shape around that number — re-check it
  after the first successful generation.
GATE: user approves the file (or chat spec).

## Stage 3: Plan
Write `plans/content/YYYY-MM-DD-<slug>.md`:
- Per-deliverable steps with model, params, full prompt draft, dependencies.
- `- [ ]` todo checkbox per deliverable.
- NodeFlow mapping (when canvas available): which steps become which nodes;
  multi-shot video uses the shot-chaining shape from `nodeflow-reference.md`
  (§ Shot chaining + stitching).
GATE: user approves the file (or chat plan).

## Stage 4: Execute
- Canvas path (nodeflow_* available): `nodeflow_create_workspace` named
  `<slug>`; build the WHOLE plan graph in one `nodeflow_apply_batch`; run
  with `nodeflow_run_and_wait` (`timeout_s` 600+ for video). **Build whole, but
  run one row first** when the inputs are filter-sensitive (any human subject):
  `from_node_id` a single shot, confirm it succeeds, then run the rest. A full
  run fails every node against the same gate and spends several escalation
  rungs before the first error is visible. The canvas has
  no project-targeting mechanism (`generatorNode` / `exportNode` expose no
  project param) — outputs land in the user's gallery/NodeFlow workspace;
  report their actual location in Stage 6.
- Direct path (no nodeflow, or trivial single deliverables): `create_project`
  (if missing), then `generate_image` / `generate_video` with
  `project_name=<slug>`.
- Seedance 2.0 video prompts: produce them via
  `../sd2-production-prompts/SKILL.md`; storyboard stills + frame-chained
  execution via `../storyboard-maker/SKILL.md`. The canvas equivalent of
  frame-chaining is lastFrameNode wiring — prefer the canvas when available.
- Tick each plan todo as its deliverable completes. Respect per-plan gates.

## Stage 5: Review & resolve
For each deliverable: present the result URL, collect feedback item by item.
To fix: **re-read the graph first** (`nodeflow_get_graph` — the user may have
tweaked nodes by hand; never clobber manual edits), change ONLY the affected
node (`nodeflow_update_node`), re-run with `from_node_id` so approved shots
are untouched. Direct-path fixes use `create_variant` (image) or a new
`generate_video`. GATE: user accepts each deliverable.

## Stage 6: Report
Append to the plan file (or send in chat):
- Deliverable table: name | final URL | credits spent.
- Total credit spend (before/after `get_credit_balance`).
- Canvas workspace name + id (canvas-path deliverables) + ProjectReverie project name (direct-path deliverables).
Then offer: iterate further, or close out.

## Failure rules
- `insufficient_credits` at any stage → stop at the current gate, report spend
  so far and the shortfall. Never retry.
- **Content-policy rejection** (e.g. `InputImageSensitiveContentDetected.…`) → stop
  the stage immediately. Do not retry the deliverable and do not continue to the next
  one. These escalate against the account (2 in 30 days suspends it), so a batch that
  keeps going converts one recoverable warning into a lockout. Report the flag, name
  the offending input, and return to the Stage 2 character-source decision.
- `Account is suspended. Generation is not permitted.` → an account-wide compliance
  gate, not a canvas or credit problem. Every tool returns it and nothing bills. Stop
  the pipeline, report which deliverables completed, and tell the user it needs an
  operator status reset or an appeal — it cannot be cleared from these tools.
- `partial` from `nodeflow_run_and_wait` → poll `nodeflow_run_status` with the
  returned task ids before reporting.
- NodeFlow tools absent → say so once, continue via direct tools.
