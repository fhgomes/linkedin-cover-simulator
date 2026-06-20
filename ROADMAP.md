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

## 1b. Real profile-photo upload for the avatar preview
The avatar circle in the stage is currently a placeholder ("you" text or a plain shape) —
it never reflects what the user actually looks like, so the keep-clear preview is more
abstract than it needs to be.
- Add a second upload control (separate from the cover upload) so users can drop in their
  **actual LinkedIn profile photo**, rendered inside the existing `.avatar` circle via
  `object-fit: cover`, the same in-memory/`FileReader`-only pattern already used for the
  cover image. No new constraint exceptions needed — this is a pure client-side read, same
  as the existing cover upload.
- This makes the "profile photo covers this" simulation close to true-to-life instead of a
  generic stand-in, which matters most for covers with faces, logos, or fine detail near
  the bottom-left.

## 1c. LLM-powered photo/cover adjustment (page calls an image AI)
A step beyond prompt generation (#1): let the user upload their current photo or cover and
have the page **call an image-generation/editing API directly** (e.g. "brighten this",
"remove the background", "match this palette") and show the result in place.
- This is a real exception to the no-network-calls constraint, not a deterministic
  template — it sends user image data to a third-party API. It must ship as an explicit,
  opt-in feature, clearly separated from the default privacy-first flow (e.g. a distinct
  "AI adjust (sends your photo to an external API)" section with its own consent toggle).
- Needs a user-supplied API key (BYO-key model, entered per session, never persisted) to
  avoid the project taking on hosting costs or key custody — store nothing beyond the
  in-memory session, mirroring the no-storage rule for everything else.
- Should clearly disclose which provider the request goes to and what's sent, before the
  first call. This is the biggest deviation from current hard constraints in this roadmap
  and needs explicit maintainer sign-off in an issue before any implementation PR.

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
Roughly in order of effort and constraint risk:
1. **#1b real avatar upload** — lowest effort, no constraint exceptions, ships independently
   of everything else.
2. **#1 deterministic prompt generation** — no network calls, moderate effort.
3. **#2 theme categories** — moderate effort, makes #3 more useful out of the box.
4. **#3 drag/compose editor** — biggest UI lift; the drag editor makes #1's prompts less
   necessary for getting placement right.
5. **#1c LLM-powered image adjustment** — biggest constraint exception (real third-party
   network calls with user image data); needs explicit maintainer sign-off before any code.

Any of these that needs a dependency, a build step, or any form of external network call
must be discussed in an issue first — see the hard constraints in `AGENTS.md` and
`CONTRIBUTING.md`.
