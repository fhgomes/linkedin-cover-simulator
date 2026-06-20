# Contributing

Thanks for considering a contribution. This project is intentionally tiny — please read
this before opening a PR.

## Before you start
1. Read `AGENTS.md` — it's the canonical doc covering architecture, the coordinate model,
   and conventions. `README.md` has the user-facing spec table. `CLAUDE.md` has notes
   specific to Claude Code, but the rules apply to any contributor, human or AI.
2. Open an issue first for anything beyond a small fix — especially new features — so we
   can agree on scope before you write code.

## Hard constraints (PRs that violate these will be declined)
- **One file.** All HTML, CSS, and JS live in `index.html`. No bundler, no framework, no
  build step.
- **No dependencies** beyond the Google Fonts `<link>` already in use.
- **No storage, no network calls, no tracking.** No `localStorage`, `sessionStorage`,
  cookies, analytics, or uploads of any kind. Uploaded images are read client-side with
  `FileReader` and never leave the browser. This is a privacy feature — keep it that way.
- **No company/Page cover support.** Different spec, out of scope for this tool.

If your change genuinely needs an exception to one of these (e.g. a real build tool), open
an issue to discuss it before submitting a PR — include the reason in the PR description if
you proceed.

## Making a change
1. Edit `index.html` directly. Layout geometry is centralized in `:root` CSS custom
   properties (`--safe-*`, `--clear-*`, `--crop-side`, `--avatar-*`, `--grid-*`) — change
   values there, never inline.
2. If your change touches a LinkedIn dimension (cover size, safe zone, crop, avatar
   position), **verify the current spec with a web search first** — LinkedIn's rendering
   has shifted before, and old assumptions (including ones baked into this repo) can go
   stale. Update the constants and the spec table in `README.md` together.
3. Keep all user-facing text in English, and keep the existing accessibility patterns
   (real `<label>`s, `alt` text on images, `aria-hidden` on decorative SVG).

## Testing your change
There's no automated test suite — this is a static file with no build step. Serve it
locally and verify manually:

```bash
python3 -m http.server 8080
# open http://localhost:8080
```

Manual QA checklist (from `AGENTS.md`) — run through this for any UI change:
1. Stage holds a clean 4:1 ratio at narrow and wide viewport widths.
2. Desktop/Mobile toggle shades only the side bands in mobile mode.
3. Guides toggle hides/shows the safe-zone and keep-clear overlays.
4. Upload renders an image with `object-fit: cover`; Clear restores the grid + motif.
5. Headline/sub-line edits update live; dark-photo and light-text toggles work.
6. No console errors; no network requests beyond the font CSS (check devtools Network tab).

## Submitting a PR
- Keep PRs small and focused — one change per PR.
- Describe what changed and why, and confirm you ran the manual QA checklist above.
- If you changed a LinkedIn spec constant, link the source you used to verify it.
- Update `README.md` if the change affects behavior, specs, or features visible to users.

## Good first issues
See the "Good first tasks" / "Roadmap" sections in `AGENTS.md` and `README.md` for ideas
(theme presets, draggable text with coordinate readout, export-to-PNG, shareable URL-hash
config, etc.).
