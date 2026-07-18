SeedEdit 3.0 is an image editing model that supports modifying images through text instructions. It performs exceptionally well in scenarios such as portrait editing, background alteration, and viewpoint or lighting transformation. This guide documents how to prompt SeedEdit 3.0 and call it via the API.

# Supported model

| Model Name | Version | Model ID | Capability | Max images / min | Price (USD / image) | Free credit |
|---|---|---|---|---|---|---|
| bytedance-seededit-3.0-i2i | 250628 (recommended) | `seededit-3-0-i2i-250628` | Image editing | 500 | 0.03 | 200 pieces |

# API parameters

SeedEdit uses the same `client.images.generate(...)` entry point as Seedream, but with a restricted parameter surface:

| Parameter | Type | Required | Notes |
|---|---|---|---|
| `model` | string | yes | `"seededit-3-0-i2i-250628"` |
| `prompt` | string | yes | The edit instruction — what should change. |
| `image` | string | yes | Public URL (`https://...`) or Base64 data URI (`data:image/<fmt>;base64,<...>`). JPEG or PNG, ≤10 MB, aspect ratio between 1/3 and 3, total pixels ≤ 36,000,000. |
| `size` | string | yes | Must be `"adaptive"` — SeedEdit matches the input aspect ratio and snaps to a preset output dimension. No custom dimensions. |
| `response_format` | string | no | `"url"` (24-hour signed link, default) or `"b64_json"`. |
| `seed` | int | no | `-1` to `2147483647`. Pin this for reproducible edits. |
| `guidance_scale` | float | no | 1.0–10.0, default `5.5`. Higher = more faithful to the edit instruction; lower = more creative interpretation. |
| `watermark` | bool | no | Default `true` in the raw API; `false` in the Reverie MCP tool. |

The response shape matches Seedream's: `data[0].url` contains the edited image (24-hour signed URL) and `usage.output_tokens` carries token usage if reported.

# Python usage

```python
import os
from byteplussdkarkruntime import Ark

client = Ark(
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3",
    api_key=os.environ.get("ARK_API_KEY"),
)

resp = client.images.generate(
    model="seededit-3-0-i2i-250628",
    prompt="Make the bubbles heart-shaped",
    image="https://ark-doc.tos-ap-southeast-1.bytepluses.com/seededit_i2i.jpeg",
    response_format="url",
    size="adaptive",
    seed=123,
    guidance_scale=5.5,
    watermark=True,
)
print(resp.data[0].url)
```

# Prompting tips

1. **Name the change, not the whole scene.** SeedEdit preserves anything you don't mention. "Make the bubbles heart-shaped" is better than re-describing the full image.
2. **Anchor fixed elements when needed.** If the model is drifting on things you want to keep, say so explicitly: "Change the background to a beach sunset, keep the subject's pose and outfit unchanged."
3. **Be concrete about what to swap.** "Replace the red car with a blue motorcycle" beats "change the vehicle." Vague pronouns ("that thing", "it") are a common failure mode.
4. **For style transfers, name the target style.** "Convert this photo to a Studio Ghibli-style watercolor" lands better than "make it look nicer".
5. **Tune `guidance_scale` when the result drifts.**
   - 3.0–4.5 → subtle, conservative edits; preserves more of the original.
   - 5.5 (default) → balanced.
   - 7.0–9.0 → aggressive compliance; use when the edit isn't being applied, accepting some image distortion.
6. **Pin a `seed` for iterative refinement.** When trying multiple wording variants, holding `seed` constant makes the comparison meaningful.

# Application scenarios

| Scene | Description |
|---|---|
| Advertising design | Generate creative variants from a hero image — adjust lighting, backgrounds, or props without re-shooting. |
| E-commerce | Swap product backgrounds, change contexts, or add lifestyle scenes around the same product. |
| Portrait editing | Change hair, outfits, backdrops, or lighting while preserving facial identity. |
| Film / game concept art | Iterate on viewpoints, times of day, or mood on an existing keyframe without starting over. |

# Notes

- SeedEdit is **synchronous** — results are returned on the same API call. No task polling.
- Charges apply only to successfully generated images. Safety-rejected requests are not charged.
- To disable the content filter, see the ModelArk Content Pre-filter documentation in your vendor console.
