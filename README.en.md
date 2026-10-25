#  EVA-UI-Pannel

**ACTIVE TIME REMAINING — Front View**

An Evangelion-inspired armored HUD countdown / clock. Pure HTML + CSS + inline SVG, zero build step, just open the file.

---

## ✨ Features

- **Charging armor background** — pitch black when off; sweeps in from bottom-left to top-right on power-on via `mask-image` translation
- **Two modes** — countdown `5:00.00` (centisecond precision) and live 24H / 12H clock, cycled by clicking the small block left of the title
- **Danger / zero states** — turns blood-red and blinks for the final 7 seconds; on zero, the title and DANGER box blink on independent rhythms, only the STOP bar remains
- **Cyan lines light up with power** — armor ticks and separator lines ignite sequentially using staggered `--d` delays
- **CRT power-off feel** — fade-in on, instant cut-off off; font falls back gracefully
- **Full keyboard accessible** — switch, mode boxes and buttons all `tabindex=0` + `:focus-visible` outline, no stray click rectangles

## 📁 Structure

```
.
├── 活動限界まで.html            # single self-contained entry point
└── font/
    └── EVA-Matisse_Classic.ttf  # title font (place it yourself)
```

## 🚀 Quick Start

```bash
git clone https://gitee.com/AmadeusC204/eva-ui-pannel.git
cd src
ls font/EVA-Matisse_Classic.ttf   # make sure the font is in place
open 活動限界まで.html            # double-click to run
```

Or use a static server (more reliable font loading):

```bash
python3 -m http.server 8080
# open http://localhost:8080/活動限界まで.html
```

## 🎮 Usage

| Action | Effect |
|---|---|
| Click `POWER SUPPLY` | Toggle main power: armor charges, screen lights up |
| Click the small block left of the title | Cycle: countdown → 24H clock → 12H clock |
| Click `INTERNAL / EXTERNAL` | Switch activity mode; active box shows zebra stripes |
| Click `SLOW / NORMAL / RACING` | Pick speed; red bar under the button marks the active gear |
| Countdown hits zero | Auto-locks to `STOP`, title + DANGER box blink in alternation |
| Tab + Enter | Full keyboard control |

> While powered off, the digits, mode boxes, buttons and DANGER box all hide — only the black armor silhouette stays.

## 🧠 State Machine

The UI is driven entirely by classes on `<body>`; just add / remove them to change state:

| class | Meaning | Triggered by |
|---|---|---|
| `pw` | Main power on: charge animation + UI fades in | POWER SUPPLY click |
| `clk` | Clock view (hides countdown, shows HH:MM:SS) | title-block click |
| `danger` | Final 7 s: amber → blood red, cyan lines stay | countdown ≤ 7s |
| `zero` | Limit reached: group A/B blink out of phase, only STOP bar left | countdown = 0 |

```js
document.body.classList.add('pw');      // power on
document.body.classList.toggle('clk'); // toggle clock
document.body.classList.add('danger'); // enter danger
document.body.classList.add('zero');   // lock at zero
```

## 🔤 Font

The title uses **EVA Matisse Classic** (self-hosted via `@font-face`). It is a closed commercial font, so **it is NOT included in this repo**:

- Drop `EVA-Matisse_Classic.ttf` into the `font/` folder yourself
- Missing? The browser falls back to `serif` — layout stays intact, but you lose the EVA glyph flavor

## 🎨 Customization

All visual tokens sit at the top of `<style>`; change one, the whole UI follows:

```css
body{
  --amber:#ffac1e;   /* primary color */
  --red:#ff2415;     /* danger red */
  --cyan:#3fd8ff;    /* tick cyan */
}
```

| What to tweak | Where |
|---|---|
| Initial countdown (default 5:00.00) | initial-seconds constant in the inline JS |
| Danger threshold (default 7s) | the `danger` check in JS |
| Canvas size (default 1300×600) | `.panel { width / height }` |
| Digit brightness | `.seg{opacity:.07}` → `.seg.on{opacity:1}` |

## 📄 License

Interface code: MIT © 2026.

`EVA-Matisse_Classic.ttf` is the property of its original author / publisher. Do not redistribute it with this repo; make sure you have a valid license before using it.

---

> *「活動限界まで — あと 5:00.00」*
