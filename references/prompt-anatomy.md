# Prompt anatomy and the five failure modes

Based on the official OpenAI cookbook guidance for GPT image models, snapshot 8 September 2026. What is here comes from documentation, not from forum folklore.

## How the model reads a prompt

The order the documentation recommends: **background and scene, then subject, then key details, then constraints.**

For anything complex, use short labeled segments or line breaks instead of one long paragraph.

Format is not the deciding factor. Minimal prompts, descriptive paragraphs, JSON-like structures, instruction style and tag style all work when the intent is clear. For repeatable production, a skimmable template beats clever syntax.

## Specificity

- name materials, shapes, textures and the medium (photo, watercolor, 3D render)
- add quality levers deliberately: `film grain`, `textured brushstrokes`, `macro detail`
- write `photorealistic` explicitly when you want photo mode
- for people, state scale, body framing, gaze and object interaction: `full body visible, feet included`, `hands naturally gripping the handlebars`

## Composition

- framing and viewpoint: close-up, wide, top-down
- angle: eye-level, low-angle
- light and mood: soft diffuse, golden hour, high-contrast
- explicit placement: `logo top-right`, `subject centered with negative space on left`

## Text inside the image

- put literal text in **quotes** or **ALL CAPS**
- specify typography as a constraint: type style, size, color, placement
- spell difficult words letter by letter
- use `quality="medium"` or higher for small text, dense information panels and multi-font layouts
- demand `verbatim rendering, no extra characters`
- **accented alphabets are the usual breaking point.** Diacritics get dropped or duplicated, and long words fail more often than short ones. When the text must be exact to the character, set it outside the model

## Style transfer

State separately **what must stay** (the style cues) and **what changes** (the new content). Add hard constraints: background, framing, `no extra elements`. Without them the style leaks into the subject matter.

## Photorealism

Use photography language (lens, lighting, framing) and explicitly ask for real texture: pores, wrinkles, fabric wear, imperfections. Avoid wording that implies studio polish or staging.

For wide, cinematic, low-light, rain or neon scenes, add extra detail about scale, atmosphere and color, otherwise the model trades mood for surface sharpness.

Film-stock references such as `35mm film photograph`, `iPhone photo` or `film grain` work for the overall look. Detailed camera specifications are interpreted loosely.

## Transparent backgrounds

- `background="transparent"` with `output_format="png"` or `"webp"`. JPEG cannot carry transparency
- omit `output_compression` for PNG
- in the prompt, ask for an isolated subject on a fully transparent background, with no scenery, no solid backdrop, no checkerboard pattern and no cast shadow
- on every follow-up edit, restate the transparency requirement or a background creeps back in

## Editing and inpainting

- state exclusions and invariants explicitly: `no watermark`, `no extra text`, `no logos/trademarks`, `preserve identity/geometry/layout/brand elements`
- the formula is `change only X` plus `keep everything else the same`, with the **preserve list repeated in every round**
- for surgical edits, also forbid changes to saturation, contrast, layout, arrows, labels, camera angle and surrounding objects

## References

- give each input an index and a role: `Image 1: product photo... Image 2: style reference...`
- describe the interaction: `apply Image 2's style to Image 1`
- when compositing, say what moves where: `put the bird from Image 1 on the elephant in Image 2`

## Quality against latency

- `low` for latency-sensitive and high-volume runs, and for finding a direction
- `medium` or `high` for small or dense text, detailed infographics, close-up portraits, identity-sensitive edits and large outputs
- `xhigh` or `max` (2.5 only) for dense typography and long information panels

## Five failure modes and their fixes

| Failure | Fix |
|---|---|
| **Drift across iterations**, each round further from the brief | return to a clean base prompt and make single-change follow-ups: `make lighting warmer`, `remove the extra tree`, `restore the original background` |
| **Broken text** | keep the prompt strict and iterate with small wording and layout tweaks, not with a longer prompt |
| **Identity lost during an edit** | lock the person explicitly (face, body shape, pose, hair, expression) and allow changes **only** to garments. On legacy models, `input_fidelity="high"` |
| **Style inconsistency across a series** | use the previous output as an anchor and restate the same cues verbatim each round: `the same green hooded tunic`, `same facial features` |
| **Background contaminating a cutout** | never mention scenery, solid backdrops or checkerboards, and repeat the transparency instruction in every edit |

## Templates by job type, from the cookbook

- **Infographic or diagram**: title, goal, required components, visual language (clean, flat, labeled, readable text)
- **Photorealism**: photography mode, subject, texture and imperfection detail, composition (framing, lens, lighting), mood, exclusions such as no glamorization and no retouching
- **Advertising**: brand positioning, audience, scene, exact copy in quotes, visual mood, exclusions such as no clip art, no stock photography, no unrelated logos
- **UI mockup**: product context, layout hierarchy, real interface elements, design system, content placement
- **Logo**: brand personality, a simplicity requirement, technical constraints (vector-like, flat, no gradients), transparent background, exclusions
- **Edit with preservation**: the change statement, an explicit preserve list, technical constraints such as no saturation or contrast change, locked camera angle, matched shadows
