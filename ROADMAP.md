# Roadmap

Forward-looking ideas for this project, beyond the "Good first tasks" already listed in
`AGENTS.md` and `README.md`. These are bigger, more speculative bets — discuss in an issue
before starting any of them, since some conflict with the current hard constraints
(single-file, zero-dependency, no network calls) and would need an explicit decision to
relax those rules for a specific feature.

## 1. LLM-assisted prompt generation
Today the tool only *validates* a cover; it doesn't help generate one. A natural extension:
- A panel that takes a few inputs (role, industry, tone, headline/sub-line) and produces a
  ready-to-paste **image-generation prompt** (for Midjourney, DALL·E, etc.), following the
  geometry rules already encoded here (safe zone, keep-clear, mobile crop) so the generated
  image respects them from the start.
- This does **not** require calling an LLM API from the browser — it can be a deterministic
  prompt template filled in client-side with the user's inputs. That keeps the no-network
  constraint intact.
- A stretch version could optionally call a user-supplied API key directly from the browser
  (still no server, no storage of the key beyond the session) — would need an explicit
  opt-in toggle and a clear "your key, your request, nothing stored" disclaimer.

## 2. Style guidelines + curated theme categories
Right now theming is just `:root` CSS variables. Worth building:
- A small set of **prepared categories** (e.g. "clean-tech", "dark-premium", "fintech",
  "creative/bold") each bundling a coherent palette, font pairing, and copy tone suggestion.
- A **palette picker** with contrast-checked combinations (text vs. avatar background, text
  vs. photo overlay) so users don't accidentally pick illegible combos.
- Guidelines docs (could live in `README.md` or a new `STYLE_GUIDE.md`) describing what
  makes a cover read well at LinkedIn's actual render size — information density, contrast,
  font scale — distilled from the existing safe-zone/keep-clear logic.

## 3. Canva-style drag/compose editor
The biggest lift: let users actually compose the cover here instead of only previewing an
upload.
- Drag-and-resize text blocks (headline, sub-line, additional badges) with snapping to the
  safe zone, live px-coordinate readout (already on the roadmap in `AGENTS.md`).
- Layer an uploaded image *underneath* draggable text/shape elements instead of replacing
  the whole canvas, so text and image compose live.
- This likely needs a small interaction/animation layer (e.g. a lightweight drag library or
  hand-rolled pointer-event dragging) rather than a full framework — keep evaluating
  "vanilla JS + a few hundred lines" vs. pulling in a dependency. Any dependency choice is a
  deviation from the current zero-dependency rule and should be called out explicitly in
  the PR, with the trade-off justified (bundle size, no build step required, etc.).
- Natural pairing with the existing "Export guide overlay" roadmap item: once composition
  is in-canvas, exporting the whole canvas (not just the guide overlay) to PNG becomes
  possible via `<canvas>` + `toDataURL`, fully client-side.

## Sequencing
These three are independent but build on each other naturally: theme categories (#2) make
the drag editor (#3) more useful out of the box, and the drag editor makes LLM-generated
prompts (#1) less necessary for getting text placement right — though prompt generation
is the lowest-effort one to ship first.

Any of these that needs a dependency, a build step, or any form of external network call
must be discussed in an issue first — see the hard constraints in `AGENTS.md` and
`CONTRIBUTING.md`.
