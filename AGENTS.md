# AGENTS.md

Static marketing/landing site for Fun Bahasa (Indonesian language & literature platform). No backend, no framework — plain HTML + Tailwind v2 + Alpine.js via CDN.

## Stack
- Pages are static `.html` at repo root, each linking `css/style.css` + a per-page `js/data-*.js`.
- Tailwind CSS **v2.2.19** (not v3). Config in `tailwind.config.js`: custom blue/gray palettes, extra `3xl`/`4xl` breakpoints, `Poppins` as default `font-sans`.
- Alpine.js v2.8.1 and Bootstrap Icons are loaded from CDN in each `<head>` — not from npm.
- Content language is Indonesian (`<html lang="id">`).

## Design conventions
- Softer visual weight: headings, nav links, buttons, and footer labels use `font-semibold` instead of `font-bold` / `font-extrabold`.
- Custom shadows are defined in `tailwind.config.js`: `shadow-soft` (low-opacity, large blur) and `shadow-pop` (slightly stronger). Prefer these over the default hard shadows for cards and elevated surfaces.
- Interactive elements (cards, buttons, links, tabs, pagination arrows, social icons) use `transition duration-200 ease-out` for smooth hover/focus states.
- Cards lift on hover with `hover:-translate-y-1 hover:shadow-soft`.
- Smooth scrolling for in-page tab links is enabled globally via `html { scroll-behavior: smooth; }` in `styles.css`.

## Build
- Tailwind source directives live in `styles.css` (`@tailwind base/components/utilities/screens`).
- `npm run build` regenerates `css/style.css`, but **the script is broken on macOS/Linux**: it uses a Windows path `css\style.css`. Run it with the locally installed v2 CLI instead:
  `npx tailwindcss build styles.css -o css/style.css`
  (This uses the project's `tailwindcss` devDependency, so it stays compatible with the v2 config.)
- `css/style.css` is a ~236k-line generated file committed to git. Don't hand-edit; change `styles.css`/config, then rebuild.
- `purge: []` means the build emits the full Tailwind CSS (no tree-shaking). Enabling purge will shrink it drastically but may drop classes used only inside `js/data-*.js` strings — include those files in `content` if you do.

## Data-driven pages
- `baca_karya.html`, `kategori-*.html`, `artikel.html`, `cerpen.html`, `cerita-anak.html`, `esai.html`, `Event.html`, `artikel-detail.html`, and `karya-detail.html` render from JS data files in `js/` (`data-karya.js`, `data-kategori-puisi.js`, `data-detail-cerpen.js`, `data-detail-artikel.js`, `data-event.js`, …).
- Data is exposed as global `var` arrays (e.g. `dataPuisi`). Pages consume it via Alpine `x-data="loadPuisi()"`, where `loadPuisi()` is an inline `<script>` function.
- **Load order matters**: the `<script src="js/data-*.js">` tag must appear *before* the inline `<script>` defining the `load*()` functions, or the data is undefined.
- Detail pages select content from query params: `karya-detail.html?kategori=<slug>&id=<id>` maps the slug to `dataPuisi`/`dataCerpen`/`dataCeritaAnak`/`dataArtikel`/`dataEsai`; `artikel-detail.html?id=<id>` reads `sourceData` from `data-detail-artikel.js`. Detail objects carry a `fullContent` array of paragraphs rendered via Alpine `x-for`.

## Card layout pattern
- In `baca_karya.html`, each category card wraps its content in `flex flex-col flex-grow`, and the author row uses `mt-auto` so the person icon + author name always sit at the bottom of the card. This keeps the grid visually aligned even when excerpt lengths vary.

## Commands
- No tests, lint, or typecheck exist — `npm test` just errors. No dev server; open `.html` files directly in a browser.

## Recently updated pages
- Most root `.html` pages were refreshed with the soft-design system above. Intentionally skipped unless explicitly requested: `coba.html`, `index2.html`, `index-after-login.html` (tracked scratch/artifact pages per the Gotchas section).

## Git workflow
- `staging` is the active integration branch; `*-feature` and topic branches merge into `staging`, which eventually merges to `main` (remote HEAD).
- Tracked stray artifacts to ignore unless explicitly working on them: `staging` (a `git branch` output dump, not source), `coba.html` (scratch/"test" page), `index2.html`, `index-after-login.html`.

## Gotchas
- `README.md` is UTF-16 LE with CRLF. UTF-8 editors will corrupt it — re-save as UTF-16 or convert deliberately.
- In `tailwind.config.js`, `orange: 'FCB316'` is missing the leading `#`, so it's an invalid color and `bg-orange`/`text-orange` won't render as that hex.
