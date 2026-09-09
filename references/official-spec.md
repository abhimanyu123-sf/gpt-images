# GPT Images: hard numbers

Snapshot of the official OpenAI documentation as of **8 September 2026**, the day ChatGPT Images 2.5 shipped.

Sources: the OpenAI image generation guide, the model pages for `gpt-image-2` and `gpt-image-2.5-flare`, the OpenAI cookbook prompting guide for GPT image models, and the ChatGPT Images 2.5 announcement.

Anything in this file is quoted from documentation. Anything not in this file is unverified, so say so instead of guessing. Model catalogs move fast: re-check before relying on a number months from now.

## Models

| Model ID | Snapshot | Role |
|---|---|---|
| `gpt-image-2.5-flare` | `gpt-image-2.5-flare-2026-09-08` | default, everyday and high-volume work, about 50% lower latency than Images 2.0 |
| `gpt-image-2.5-sunburst` | not published | precision across editing rounds, campaign creative, longer generation times |
| `gpt-image-2` | `gpt-image-2-2026-04-21` | previous generation, still supported |
| `gpt-image-1.5`, `gpt-image-1`, `gpt-image-1-mini` | | legacy, the only models with `input_fidelity` |

Access to GPT Image models may require API Organization Verification in the developer console.

## Endpoints

- `v1/images/generations`
- `v1/images/edits`, including masked inpainting
- Responses API, tool `image_generation`, with `action` set to `auto` (default), `generate` or `edit`
- `v1/batch` supports `gpt-image-2`. The 2.5 models have no batch support.

## Parameters

| Parameter | Values | Notes |
|---|---|---|
| `size` | `1024x1024`, `1536x1024`, `1024x1536`, or custom `WIDTHxHEIGHT` | see the size rules below |
| `quality` | `low`, `medium`, `high`, `xhigh`, `max`, `auto` | `xhigh` and `max` are 2.5 only, default is `auto` |
| `background` | `transparent`, `opaque`, `auto` | `transparent` needs `png` or `webp`, `opaque` is new in 2.5 |
| `output_format` | `png` (default), `jpeg`, `webp` | jpeg has lower latency |
| `output_compression` | 0 to 100 | jpeg and webp only |
| `moderation` | `auto` (default), `low` | |
| `n` | 1 or more | multiple images per request |
| `partial_images` | 0 to 3 | progressive streaming |
| `input_image_mask` | file reference | Responses API |
| `input_fidelity` | `low`, `high` | `gpt-image-1.5` and `gpt-image-1` only. On `gpt-image-2` inputs are processed at high fidelity automatically and the parameter is disabled |

## Size rules for the 2 and 2.5 families

- every edge a **multiple of 16**
- longest edge **below 3840 px**
- aspect ratio at most **3:1** in either direction
- total pixels between **655,360 and 8,294,400**
- above `2560x1440` (2K) results are **experimental and variable**
- 4K UHD `3840x2160` fails the multiple-of-16 rule on height, use `3824x2144`

Reliable sizes: `1024x1024`, `1536x1024`, `1024x1536`, `2560x1440`.

## Input images and masks

- the edits endpoint accepts **up to 16 input images**, each **under 50 MB**
- png, jpeg and webp inputs
- **a mask must match the format and dimensions of the edited image and must contain an alpha channel.** It is not a black and white bitmap, transparency carries the selection

## Pricing, token based, identical for flare, sunburst and gpt-image-2

| Item | Per 1M tokens |
|---|---|
| text input | $5 |
| text cached input | $1.25 |
| image input | $8 |
| image cached input | $2 |
| image output | $30 |

Moving from `gpt-image-2` to 2.5 costs nothing extra, it is only faster.

## Rate limits for `gpt-image-2`, tokens per minute / images per minute

Tier 1: 100,000 / 5 · Tier 2: 250,000 / 20 · Tier 3: 800,000 / 50 · Tier 4: 3,000,000 / 150 · Tier 5: 8,000,000 / 250.

Limits for the 2.5 family were not published as of 8 September 2026.

## What Images 2.5 added in the ChatGPT app

- sketch input drawn directly in the conversation
- comment-based editing: click a spot on the image instead of describing it
- named templates including poster and merchandise formats
- edits carry across a conversation, each new instruction builds on the previous ones
- more natural lighting, richer textures, better preservation of subjects from reference images
- rolled out across ChatGPT tiers on desktop, mobile and web
- measured unsafe generation rates in adversarial testing: 1.09% for Sunburst, 1.41% for Flare

## Unverified as of 8 September 2026

- rate limits for the 2.5 family
- maximum prompt length. The documentation states none. The cookbook argues for short labeled blocks rather than length
- how third-party hosted surfaces map their model names onto flare and sunburst. Check the platform's own catalog
