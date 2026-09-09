# Recipes: skeletons per output type

Fill the skeleton in, do not paste it as is. Write in English and keep in-image copy verbatim in quotes.
Every size must satisfy the rules in `official-spec.md`: multiple of 16, aspect ratio within 3:1.

## Sizes by channel

| Channel | Size | Note |
|---|---|---|
| Article cover, wide social | `1920x1080` | keep the top and bottom 30% visually empty, platforms crop there |
| Wide banner, 5:2 | `1920x768` | |
| Portrait social post | `1080x1350` | |
| Story or vertical cover | `1080x1920` | 9:16 is within the 3:1 limit |
| A4 poster, print, 300 dpi | `2480x3504` | rounded to a multiple of 16, expect more attempts above 2K |
| Product shot for web | `1536x1024` or `2048x1360` | |
| Avatar or icon | `1024x1024` | |

## 1. Poster or event announcement

```
USE: A4 portrait poster, print, 2480x3504.
SUBJECT: <main motif>.
SCENE: <background>, single accent color against a dark ground.
COMPOSITION: headline occupies the top third, large negative space in the middle,
detail block bottom-left, logo bottom-right.
LIGHT & STYLE: flat graphic design, hard edges, no gradients, no pastels.
TEXT: headline "<HEADLINE IN CAPS>", subline "<subline>", details "<date, venue>".
Bold condensed geometric sans, all caps headline. Verbatim rendering, no extra characters.
AVOID: stock photo look, clip art, third-party logos, watermark, extra text, drop shadows.
```

Proofread the render band by band before anyone sees it. If the type has to be exact, generate the artwork here and set the text in a layout tool.

## 2. Article cover

```
USE: Article cover, 1920x1080, mostly viewed small on mobile.
SUBJECT: <one strong motif, not a scene with ten elements>.
COMPOSITION: subject centered in the middle horizontal band; keep the top 30% and
bottom 30% visually empty for platform cropping.
LIGHT & STYLE: <photorealistic | editorial illustration | 3D render>, high contrast,
readable as a 400px thumbnail.
TEXT: none.
AVOID: small text, thin lines, busy background, watermark, logos.
```

Prefer no text at all in a cover. At thumbnail size nobody reads it and the model breaks it anyway.

## 3. Product shot

```
USE: E-commerce product shot, 1536x1024.
SUBJECT: <product>, exact shape, label and colors as in Image 1.
SCENE: <surface and environment>, shallow depth of field.
COMPOSITION: three-quarter view, product fills 60% of the frame, negative space on the right.
LIGHT & STYLE: photorealistic, soft key light from the left, subtle reflection,
real surface texture and micro-imperfections.
TEXT: none.
AVOID: floating product, plastic CGI sheen, extra props, invented logos, watermark.
```

With a reference: `Image 1: product photo - keep geometry, label typography and colors exactly.`

## 4. Portrait from a reference

```
Image 1: reference portrait of the subject - keep facial features, hair and build exactly.
USE: <purpose>.
SUBJECT: the person from Image 1, <clothing and situation>.
SCENE: <environment>.
COMPOSITION: <framing>, eye-level.
LIGHT & STYLE: photorealistic, natural skin texture with visible pores and fine lines,
no beauty retouching, no skin smoothing.
AVOID: altering face shape, age, hairline or expression; glamour lighting; watermark.
```

Use a color reference, not a stylized or black and white one, or the model inherits the treatment. Depicting a real person is exactly the case where AI-content disclosure rules apply, so check what your jurisdiction requires and get the person's consent.

## 5. Infographic or diagram slide

```
USE: Presentation slide graphic, 1536x1024, projected.
SUBJECT: a clean diagram showing <what> in <N> steps.
COMPOSITION: horizontal flow left to right, equal spacing, one accent color for the active step.
LIGHT & STYLE: flat vector, thick strokes, no 3D, no shadows, white background.
TEXT: step labels "<A>", "<B>", "<C>"; title "<title>".
Verbatim rendering, no extra characters, no invented labels.
AVOID: decorative icons without meaning, gradients, fake data, unreadable small text.
```

Use `high` quality, or `xhigh` for dense typography. The model invents labels when you name fewer than the number of elements you asked for, so list every one.

## 6. Logo or icon on a transparent background

```
USE: App icon or logo mark, 1024x1024, transparent background.
SUBJECT: <motif>, single mark, geometric, legible at 32px.
LIGHT & STYLE: flat vector look, solid fills, no gradients, no bevel, no drop shadow.
BACKGROUND: fully transparent, isolated subject, no scenery, no solid backdrop,
no checkerboard pattern, no cast shadow.
AVOID: text, trademarked shapes, photographic detail, multiple marks.
```

API: `background: "transparent"` with `output_format: "png"`. Hosted surfaces often return an opaque image, in which case cut the background out afterwards with a dedicated tool.

## 7. Interface mockup

```
USE: Product UI mockup for a landing page, 1536x1024.
SUBJECT: a <app type> dashboard on <device>.
COMPOSITION: interface fills the frame, one primary panel with <content>, sidebar on the left.
LIGHT & STYLE: clean modern design system, white surface, one accent color,
real interface elements (buttons, tabs, charts), minimal decoration.
TEXT: panel title "<text>", nav items "<A>", "<B>", "<C>". Verbatim, no lorem ipsum.
AVOID: fake unreadable text, distorted charts, browser chrome artifacts, watermark.
```

## Settings blocks

**OpenAI API**
```
model: gpt-image-2.5-flare      # sunburst for campaigns and multi-round edits
size: 1920x1080
quality: high
background: opaque              # transparent only with png or webp
output_format: png
n: 4
```

**ChatGPT app**
The prompt alone. State the size in a sentence, for example `1920x1080 landscape`. For iterating on an existing image, the app's sketch input and click-to-comment editing beat rewriting the prompt.

**Third-party hosted surface**
Read that platform's live model catalog first. Aspect ratios, resolution tiers and quality levels are the platform's own and rarely match the API one to one. Never promise a value you have not seen in its catalog.
