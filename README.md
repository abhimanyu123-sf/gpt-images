# GPT Images: a prompt-writing skill for Claude

Give it a brief in plain language. It gives back an English prompt for OpenAI GPT Image models, a settings block for the surface you are on, and three levers for the next round.

Covers ChatGPT Images 2.5 (`gpt-image-2.5-flare`, `gpt-image-2.5-sunburst`) and `gpt-image-2`.

## What is inside

| File | What it holds |
|---|---|
| `SKILL.md` | the workflow: surface, intake, model choice, six-block prompt, references, edits, handover |
| `references/official-spec.md` | hard numbers: model IDs, size rules, quality levels, input limits, pricing, endpoints |
| `references/prompt-anatomy.md` | text rendering, style transfer, transparency, five failure modes and their fixes |
| `references/recipes.md` | seven skeletons: poster, article cover, product shot, portrait, infographic, logo, UI mockup |

Self-contained. No other skill required.

## Install in Claude Code

```bash
git clone https://github.com/lukasersil/gpt-images.git ~/.claude/skills/gpt-images
```

That makes it available in every project. For one project only, clone into `.claude/skills/gpt-images` inside that project instead.

Copy the **whole folder**, not just `SKILL.md`. The references are half the skill, and a folder with only the top-level file loads and then quietly underperforms.

## Install on claude.ai

Download `gpt-images.zip` from [Releases](https://github.com/lukasersil/gpt-images/releases), then go to Settings, Capabilities, and upload it.

Do not unzip and rezip it through Finder. macOS adds a wrapper folder and a `__MACOSX` sidecar, and the uploader rejects the result with *"SKILL.md file must be in the top-level folder, not nested deeper."* If you have to repack, use `zip -rq gpt-images.zip gpt-images -x "*.DS_Store"` from a terminal.

## How to use it

Just describe what you need:

- "poster for an October workshop, A4, dark background"
- "product shot of this bottle on wet stone"
- "change only the jacket color, keep everything else"
- "logo on a transparent background, has to work at 32px"

The skill asks for whatever is missing, assumes the rest and marks its assumptions.

**It does not generate images.** It writes the prompt and then asks. Generation costs money and that call is yours.

## One caveat

The numbers in `references/official-spec.md` are a snapshot from 8 September 2026, the day Images 2.5 shipped. Model catalogs move fast. Before betting a production pipeline on a limit, check the current OpenAI documentation. The prompting method in `SKILL.md` and `references/prompt-anatomy.md` ages far more slowly than the parameter table.

Pull requests that update the spec against newer documentation are welcome. Keep the "Unverified" section honest: if the docs are silent, say so instead of guessing.

## License

MIT. See `ATTRIBUTION.md` for sources.
