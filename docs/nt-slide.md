# NT-Slide

`nt-slide.skill` packages the reusable NT presentation style based on the 16-slide “พื้นฐาน LLM” reference deck.

## What it does

- Uses NT yellow, charcoal, white, and warm gray on a 16:9 slide canvas.
- Enlarges primary and secondary text by at least 2.5× when restyling an existing deck; simplifies or splits dense content to keep it readable.
- Places the original NT logo on every slide.
- Uses ImageGen for professional, slide-specific visuals, with titles and other exact copy set separately as accurate slide text.
- Includes the reference cover, a reference content slide, and the NT logo as bundled assets.

## Install in Codex

Extract the `.skill` archive into a folder named `nt-slide` under the Codex skills directory:

```sh
mkdir -p ~/.codex/skills/nt-slide
unzip nt-slide.skill -d ~/.codex/skills/nt-slide
```

The resulting entry point is `~/.codex/skills/nt-slide/SKILL.md`. Invoke it with `$nt-slide` or ask for an NT-style slide deck.

## Package contents

- `SKILL.md` — style rules and presentation workflow
- `agents/openai.yaml` — UI metadata for the skill
- `assets/nt-logo.png` — supplied NT logo
- `assets/reference-cover.png` — cover-slide style reference
- `assets/reference-content.png` — content-slide style reference
