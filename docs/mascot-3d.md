# Mascot 3D

`mascot-3d.skill` packages the animated 3D mascot of the NT AI website (น้อง Connect) as a kit for any brand. You give it a logo; it puts the logo on the mascot, matches the colours, and installs it on a website or chat widget.

## What it does

- Renders a glossy 3D robot with Three.js, built from primitives, so it needs no model files.
- Changes the logo (chest plate and badge), 12 colour slots, the name, and the chest style (logo or NT-style voice bars) at runtime. One prebuilt bundle serves every brand.
- Takes the body and accent colours from the logo automatically (`autoColors`), or uses brand-guide hex values.
- Lip-syncs to speech: TTS audio, a microphone, or the host's own audio analysis.
- Follows the chat state: searches with a magnifying glass while thinking, gestures while talking, hops with a party hat on success.
- Has about 20 activities with props (juggling, bubble gum, flying, hula hoop, selfie, matcha, dumbbells, phone call…), naps when nobody is around, and reacts to clicks.
- Includes Mascot Studio, a page to try a logo and colours, play every move, and export `brand.json`.

## Install

The archive holds one `mascot-3d/` folder.

Claude Code:

```sh
unzip mascot-3d.skill -d ~/.claude/skills/
```

Codex:

```sh
unzip mascot-3d.skill -d ~/.codex/skills/
```

claude.ai or Claude Desktop: upload `mascot-3d.skill` in the Skills settings.

The entry point is `mascot-3d/SKILL.md`. The bundles in `dist/` are prebuilt; `npm install` is only needed to change the source in `src/`.

## Use

Ask for a 3D mascot with your logo, for example:

- "ทำมาสคอต 3D ใส่โลโก้นี้ ลงหน้าเว็บ" (attach the logo)
- "เอาน้อง Connect ไปใช้กับเว็บ Aqua เปลี่ยนโลโก้และสีตามแบรนด์"
- "Add an animated 3D assistant mascot with our logo to the chat widget in this React app"

The skill previews the mascot in Mascot Studio with your logo, shows a screenshot, then installs it in the project and wires it to the chat state and voice.

To try it by hand, serve the `dist/` folder over http and open `studio.html`:

```sh
cd ~/.claude/skills/mascot-3d/dist && python3 -m http.server 8080
```

Then open http://localhost:8080/studio.html, upload a logo, tune the colours, and download `brand.json`.

On a page, it takes a container and a brand:

```html
<div id="mascot" style="width:440px;height:480px"></div>
<script src="mascot3d.js"></script>
<script>
  Mascot3D.createMascot(document.getElementById("mascot"), {
    brand: { name: "Aqua Bot", logo: "aqua-logo.png", badgeText: "AQUA", emblem: "logo", autoColors: true },
  });
</script>
```

The logo and any TTS audio must be served from the same site, or with CORS.

## Package contents

- `SKILL.md`: workflow from logo to installed mascot, brand config, gotchas
- `dist/mascot3d.js`, `dist/mascot3d.mjs`: ready bundles (script tag / ES module, three.js included, about 140 KB gzipped)
- `dist/studio.html`: Mascot Studio
- `dist/nt-logo.png`: the NT logo, the studio's default
- `src/`: TypeScript source (brand, model, engine, activities, props, voice)
- `scripts/build.mjs`: rebuilds `dist/` from `src/`
- `references/api.md`: options, methods, colour slots, activities, moves and cues
- `references/integration.md`: plain HTML, React/Next/Vue, lip-sync, speech bubbles, phones, the NT web-chat widget
- `references/customizing.md`: anatomy, pose channels, adding moves, activities and props

## Source

Maintained in `tanlull/NT-Work` under `skills/mascot-3d/` (private). After changing it there, package it again into this repository.
