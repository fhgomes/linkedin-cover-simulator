# LinkedIn Cover Simulator

Preview a LinkedIn **personal** profile cover at the exact **1584 × 396** spec before you
export it — so you never again ship a banner only to find the text hidden behind your
profile photo or chopped off on mobile.

It's a single static HTML file. No build, no dependencies, no tracking. Your text and any
uploaded image stay in the browser — nothing is sent anywhere.

## Why
Designing a LinkedIn cover in Canva or Figma is easy. Getting it to survive LinkedIn's
layout is the hard part:
- the **circular profile photo** sits over the bottom-left and hides whatever is under it;
- **mobile** crops the left/right edges, so edge content disappears on phones.

This tool overlays those constraints on a true-to-spec canvas so you can check your design
in seconds.

## Features
- Exact **1584 × 396 (4:1)** stage that scales responsively while keeping proportions.
- **Profile-photo overlay** + a shaded "keep-clear" zone (bottom-left ~568 × 264 px).
- **Safe-zone** guide (centered ~1150 × 260 px) showing where text belongs.
- **Desktop ↔ Mobile** toggle that shades the ~192 px each side mobile trims.
- **Upload** your generated cover (PNG/JPG) and preview it in place.
- Live **headline / sub-line** fields, plus toggles for dark photo / light text to test
  color harmony.
- **"Image already has text"** toggle to hide the headline/sub-line overlay when your
  uploaded cover already has text baked in.

## Run it
Just open `index.html` in any modern browser. To serve locally:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

### Deploy on GitHub Pages
Push to a public repo, then in **Settings → Pages** set the source to the `main` branch
root. `index.html` is the entry point, so the simulator is live at your Pages URL.

## How it works
All zones are expressed as percentages of the 1584 × 396 canvas, so a single
`aspect-ratio` box scales everything correctly. The geometry lives in CSS custom properties
at the top of `index.html` (`--safe-*`, `--clear-*`, `--crop-side`, `--avatar-*`,
`--grid-*`) — change those to tune the layout. See `AGENTS.md` for the full coordinate
model and contribution guidance.

## Specs reference (personal profile, 2026)
| Item | Value |
|---|---|
| Cover size | 1584 × 396 px (4:1) |
| Formats | PNG, JPG (GIF renders static) |
| Max file size | 8 MB |
| Keep-clear (photo) | bottom-left ~568 × 264 px |
| Mobile crop | ~192 px trimmed each side |

> Company/Page covers use a different spec (~4200 × 700 px) and are **out of scope** here.

## Roadmap ideas
- Theme presets (clean-tech / dark-premium / fintech) + a custom palette picker.
- Draggable text block with live coordinate readout.
- Export a guide-overlay PNG to drop into Canva/Figma.
- Side-by-side desktop/mobile view.
- Shareable config via URL hash (no storage).

## Contributing
PRs welcome — see `CONTRIBUTING.md` for the hard constraints, QA checklist, and PR process.
Keep it dependency-free and single-file unless there's a strong reason not to. Read
`AGENTS.md` (and `CLAUDE.md` if you use Claude Code) before changing layout constants —
they must stay true to current LinkedIn specs.

## License
MIT — see `LICENSE`.

## Author
Created by **Fernando Gomes** ([fhgomes](https://github.com/fhgomes)).

- YouTube: [@fhgomestech](https://www.youtube.com/@fhgomestech)
- Instagram: [@fhgomes.tech](https://www.instagram.com/fhgomes.tech/)
- LinkedIn: [in/fhgomes](https://www.linkedin.com/in/fhgomes)
- Blog: [blog.fhgomes.com](https://blog.fhgomes.com/)
- Email: [tech.fernando.gomes@gmail.com](mailto:tech.fernando.gomes@gmail.com)
