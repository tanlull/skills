---
name: thai-slide-fonts
description: >
  Make PowerPoint (.pptx) slide text EASY TO READ — font style and font size only.
  Two jobs: (1) render Thai correctly by setting the complex-script typeface (a:cs)
  that python-pptx omits, and (2) make text big enough for a projector by scaling
  sizes and enforcing a readable minimum. Works on NEW text and on EXISTING decks
  (enlarge in place). Use when the user wants Thai slide fonts fixed or bigger, says
  "ตัวหนังสือสไลด์เล็ก/อ่านยาก", "ขยายฟอนต์สไลด์", "ฟอนต์ไทยเพี้ยนในสไลด์", or wants
  readable slide type. This skill does NOT touch colors, icons, layout, or shapes.
---

# thai-slide-fonts — readable slide fonts (style + size only)

Narrow, reusable skill: only fonts. It makes slide text legible and fixes Thai
rendering. It does not impose any colors, icons, cards, or layout — use it on any
deck regardless of its design.

## Files
- `scripts/thai_fonts.py` — the library (functions below).
- `scripts/example.py` — builds a small deck and enlarges it (runnable demo).

## Install
```bash
pip install python-pptx --break-system-packages
```

## Why slides need this
1. **Thai font** — python-pptx writes only `a:latin`. Thai glyphs use the
   complex-script slot `a:cs`; if it is unset, Thai may show a substitute font or odd
   spacing in real PowerPoint. Every helper here sets `a:latin` + `a:ea` + `a:cs`.
2. **Size** — Thai (especially TH Sarabun New) looks smaller than Latin at the same
   point size, and seminar rooms need big type. So scale up and enforce a floor.

Bold/italic need no special handling — DrawingML `@b`/`@i` already apply to Thai;
only the *typeface* needs the latin/ea/cs split.

## Readable size guide (minimums for a projector)
| Element  | min pt |
|----------|--------|
| title    | 40 |
| subtitle | 28 |
| heading  | 30 |
| body / bullets | 24  (22 is the absolute floor) |
| caption / note | 18 |

These live in `READABLE` (a dict) in the module. Rule of thumb: **when you make fonts
bigger, also shorten the text** — big type + crammed slides overflow. Prefer fewer
words and split dense slides.

## API (all in `thai_fonts.py`)

**New text** — set font + size correctly as you add runs:
```python
from thai_fonts import style_run, READABLE
p = text_frame.paragraphs[0]
r = p.add_run(); r.text = "หัวข้อ"
style_run(r, font="TH Sarabun New", size=READABLE["title"], bold=True)
```
`set_thai_run(run, font=None)` is the low-level piece (set/copy typeface onto a:cs);
pass `font=None` to keep the author's Latin font and just fix Thai.

**Existing deck** — enlarge + Thai-fix in one pass (text boxes, tables, groups):
```python
from thai_fonts import enlarge, apply_fonts
enlarge("in.pptx", "out.pptx", scale=1.3, min_size=22)          # quick: +30%, floor 22pt
# full control:
apply_fonts("in.pptx", out="out.pptx",
            font="TH Sarabun New",   # or set_family=False to keep the deck's own fonts
            set_family=True,
            scale=1.5,               # multiply every explicit run size
            min_size=24,             # lift anything smaller up to 24pt
            max_size=None,
            default_size=None)       # set size on runs that inherit one (use sparingly)
```
CLI: `python scripts/thai_fonts.py in.pptx out.pptx 1.3 22`

Notes: only runs with an **explicit** size are scaled; runs that inherit their size
from a layout/placeholder are left unless you pass `default_size`. `set_family=False`
preserves each run's chosen Latin font and only repairs Thai (`a:cs`).

## QA (the sandbox usually lacks TH Sarabun New)
```bash
soffice --headless --convert-to pdf out.pptx --outdir /tmp
pdftoppm -jpeg -r 96 /tmp/out.pdf /tmp/s
```
Thai will show as boxes (□) here because the font isn't installed — that is EXPECTED,
not a defect. Confirm correctness by checking `a:cs="TH Sarabun New"` appears in
`ppt/slides/slideN.xml`. After enlarging, scan the render for text overflowing its
box or a title wrapping into content, and trim wording if so. On a machine with TH
Sarabun New it renders properly; export to PDF for recipients who may lack the font.
