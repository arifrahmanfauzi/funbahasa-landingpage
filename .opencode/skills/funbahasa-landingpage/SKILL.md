---
name: funbahasa-landingpage
description: Work on the Fun Bahasa static landing site (HTML, Tailwind v2, Alpine.js v2). Enforces design conventions, build commands, data-driven page patterns, and known gotchas.
license: ISC
compatibility: opencode
metadata:
  project: funbahasa-landingpage
  stack: html-tailwind-alpine
---

# Fun Bahasa Landing Page

## Stack & constraints
- Static `.html` pages at repo root. No backend, no framework.
- Tailwind CSS v2.2.19 via local devDependency. Config in `tailwind.config.js`.
- Alpine.js v2.8.1 and Bootstrap Icons loaded from CDN in each page `<head>`.
- Content language is Indonesian (`<html lang="id">`).

## Build
- Tailwind source directives live in `styles.css`.
- Do not use `npm run build`; the script uses a Windows path (`css\style.css`) and fails on macOS/Linux.
- Correct command: `npx tailwindcss build styles.css -o css/style.css`
- `css/style.css` is generated and committed; do not hand-edit it.

## Design conventions
- Use `font-semibold`, not `font-bold` / `font-extrabold`, for headings, nav links, buttons, and footer labels.
- Prefer custom shadows defined in `tailwind.config.js`: `shadow-soft`, `shadow-pop`.
- Add `transition duration-200 ease-out` to interactive elements (cards, buttons, links, tabs, pagination arrows, social icons).
- Cards lift on hover: `hover:-translate-y-1 hover:shadow-soft`.
- Smooth scrolling is set globally in `styles.css`.

## Data-driven pages
- `baca_karya.html`, `kategori-*.html`, `artikel.html`, `cerpen.html`, `cerita-anak.html`, `esai.html`, and `Event.html` render from `js/data-*.js`.
- Data is exposed as global `var` arrays (e.g. `dataPuisi`).
- Load order matters: `<script src="js/data-*.js">` must appear BEFORE the inline `<script>` that defines the `load*()` functions.

## Card layout pattern
- In grids, wrap card content with `flex flex-col flex-grow` and place the author row with `mt-auto` so the person icon + author name always sits at the bottom of the card.

## Git workflow
- Active integration branch is `staging`; feature/topic branches merge into `staging`, then `staging` merges to `main`.
- Ignore stray artifacts unless explicitly requested: `staging` (a branch-output dump), `coba.html`, `index2.html`, `index-after-login.html`.

## Gotchas
- `README.md` is UTF-16 LE with CRLF; editing with UTF-8 tools corrupts it.
- `tailwind.config.js` defines `orange: 'FCB316'` missing the leading `#`, so `bg-orange` / `text-orange` will not render as that hex.
- `npm test` only errors; there are no tests.
- No dev server; open `.html` files directly in a browser.
