# AGENTS.md

Guidance for AI coding agents (and humans) continuing this project. This is the canonical
contributor doc; `CLAUDE.md` defers to it.

## Purpose
A single-file, zero-dependency web tool that previews a LinkedIn **personal** profile cover
at the exact **1584 × 396** spec, overlaying the two things that break real covers: the
**profile-photo safe zone** (bottom-left) and the **mobile edge crop**. The goal is to let
someone verify a design — or a freshly generated image — before exporting and uploading.

It originated as the interactive companion to a LinkedIn/CV prompt suite (a banner-prompt
module that hands users copy-paste image-generation prompts). Keep that lineage in mind:
this tool *validates* covers; it does not generate them.

## Architecture (intentionally minimal)
- **One file:** `index.html` contains all HTML, CSS, and JS. No framework, no bundler.
- **No dependencies** except Google Fonts (Inter, JetBrains Mono) loaded via `<link>`.
- **No persistence, no network calls.** Uploaded images are read with `FileReader` and held
  in memory only. Do **not** add `localStorage`, `sessionStorage`, cookies, analytics, or
  any upload/telemetry. Privacy ("nothing leaves the browser") is a feature.
- **No build step.** Open the file or serve it statically.

## Coordinate model (read before touching layout)
Every zone is a percentage of the 1584 × 396 canvas. A single element with
`aspect-ratio: 1584/396` is the stage; children are positioned with `%` so the whole thing
scales responsively without breaking proportions. The constants live in `:root` CSS custom
properties at the top of `index.html`:

| Token | Meaning | Default | Derivation |
|---|---|---|---|
| `--safe-left/top/w/h` | safe zone (place text here) | 13.7% / 17.2% / 72.6% / 65.7% | ~1150×260 centered |
| `--clear-w/h` | bottom-left photo keep-clear | 35.9% / 66.7% | ~568×264 |
| `--crop-side` | mobile crop each side | 12.12% | ~192/1584 |
| `--avatar-w/left/bottom` | profile-photo circle | 15% / 3% / -9% | visual approximation |
| `--grid-x/y` | 80px reference grid | 5.05% / 20.2% | 80/1584, 80/396 |

If you change a layout number, change it here, not inline. Then **verify it against current
LinkedIn specs** — LinkedIn occasionally adjusts rendering (the avatar and bottom crop
shifted in 2026). When in doubt, search for the current personal-profile cover spec rather
than trusting a hardcoded value.

## Run / test
```bash
python3 -m http.server 8080   # open http://localhost:8080
```
There is no test suite. Manual QA checklist for any UI change:
1. Stage holds a clean 4:1 ratio at narrow and wide widths.
2. Desktop/Mobile toggle shades only the side bands in mobile mode.
3. Guides toggle hides/shows safe + keep-clear overlays.
4. Upload renders an image with `object-fit: cover`; Clear restores the grid + motif.
5. Headline/sub-line edits update live; dark-photo and light-text toggles work.
6. "Image already has text" toggle hides the headline/sub-line overlay.
7. No console errors; no network requests beyond the font CSS.

## Conventions
- Vanilla JS, `const`/`let`, small `$()` helper. No frameworks, no new deps without a strong
  reason and a note in the PR.
- Keep accessibility: real labels, `alt` text, decorative SVG marked `aria-hidden`.
- All user-facing copy in **English**.
- Identity/theme tokens (`--bg`, `--ink`, `--accent`, `--line`) are the clean-tech default;
  keep them as variables so theming stays trivial.

## Bigger ideas
See `ROADMAP.md` for larger, more speculative bets (LLM-assisted prompt generation, curated
theme categories, a Canva-style drag/compose editor) — some would need an explicit decision
to relax the no-dependency / no-network constraints below. Discuss in an issue first.

## Good first tasks
- Theme presets (clean-tech / dark-premium / fintech) via swapping the `:root` token block.
- Custom palette + background-color picker.
- Draggable text block with a live px-coordinate readout (offer "is this inside the safe
  zone?" feedback).
- "Export guide overlay" button (canvas → PNG) to drop into Canva/Figma.
- Encode current settings in the URL hash for shareable previews (still no storage).

## Definition of done
- Single-file, dependency-free, no storage/telemetry.
- Geometry constants centralized and verified against current LinkedIn specs.
- Manual QA checklist passes; README updated if behavior or specs changed.
