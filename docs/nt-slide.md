# NT-Slide

`nt-slide.skill` packages the reusable NT presentation style based on the 16-slide “พื้นฐาน LLM” reference deck.

## What it does

- Uses NT yellow, charcoal, white, and warm gray on a 16:9 slide canvas.
- Enlarges primary and secondary text by at least 2.5× when restyling an existing deck; simplifies or splits dense content to keep it readable.
- Places the original NT logo on every slide.
- Generates one complete 16:9 slide page with ImageGen for each slide, then composes the pages into PowerPoint.
- Adds the original NT logo separately to every slide. If ImageGen distorts text, regenerate the page or correct it with accurate overlays before delivery.
- Keeps the slide text editable as PowerPoint overlays when requested.
- Includes the reference cover, a reference content slide, and the NT logo as bundled assets.

## Install in Codex

Extract the `.skill` archive into a folder named `nt-slide` under the Codex skills directory:

```sh
mkdir -p ~/.codex/skills/nt-slide
unzip nt-slide.skill -d ~/.codex/skills/nt-slide
```

The resulting entry point is `~/.codex/skills/nt-slide/SKILL.md`. Invoke it with `$nt-slide` or ask for an NT-style slide deck.

## Package contents

- `SKILL.md` — full-page ImageGen workflow, NT style rules, and PowerPoint assembly steps
- `agents/openai.yaml` — UI metadata for the skill
- `assets/nt-logo.png` — supplied NT logo
- `assets/reference-cover.png` — cover-slide style reference
- `assets/reference-content.png` — content-slide style reference
