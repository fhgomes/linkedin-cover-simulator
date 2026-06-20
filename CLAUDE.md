# CLAUDE.md

Guidance for Claude Code working in this repo. **Read `AGENTS.md` first** — it's the
canonical contributor doc. This file adds Claude-specific workflow notes; where the two
overlap, `AGENTS.md` wins.

## TL;DR
- One file: `index.html`. No build, no deps (except Google Fonts), **no storage, no network
  calls**. Keep it that way.
- Layout geometry lives in `:root` CSS variables. Edit there, never inline.
- Before changing any LinkedIn dimension, **verify the current spec via web search** —
  LinkedIn's render shifts over time and your training data may be stale.

## Project in one line
A static, single-file simulator that previews a LinkedIn personal cover at the exact
1584 × 396 spec, overlaying the profile-photo safe zone and the mobile crop so a design can
be validated before export. It validates covers; it does not generate them.

## Workflow for Claude Code
1. Read `AGENTS.md` (architecture, coordinate model, conventions, QA checklist).
2. Make the smallest change that satisfies the request. Prefer editing CSS variables and
   small JS handlers over restructuring.
3. If a change touches LinkedIn dimensions or crop math, run a web search to confirm the
   current personal-profile spec, then update the constants **and** the spec table in
   `README.md` together so they don't drift.
4. Manually verify against the QA checklist in `AGENTS.md` (open `index.html`). There is no
   automated test suite.
5. Update `README.md` if behavior or specs changed.

## Hard constraints (do not violate)
- No `localStorage` / `sessionStorage` / cookies / analytics / uploads / external API calls.
  In-browser only is a privacy feature, not an oversight.
- No frameworks or build tooling without an explicit request and a PR note.
- Keep all user-facing text in English and keep accessibility (labels, `alt`, `aria-hidden`
  on decorative SVG).
- Don't add company-Page support here (different spec, out of scope).

## Where things are
- Theme tokens: `:root { --bg, --ink, --accent, --line, --grey }`.
- Geometry tokens: `:root { --safe-*, --clear-*, --crop-side, --avatar-*, --grid-* }`.
- Behavior: the `<script>` at the bottom (text binding, device toggle, guides, tone toggles,
  upload/clear).

## Commands
```bash
python3 -m http.server 8080   # serve locally, open http://localhost:8080
```

## Roadmap (pick from these or AGENTS.md)
Theme presets · custom palette picker · draggable text with coordinate readout · export a
guide-overlay PNG · shareable settings via URL hash (still no storage).
