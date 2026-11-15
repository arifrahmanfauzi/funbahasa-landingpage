---
name: tailwindcss
description: Tailwind CSS v2.2.19 usage guide for the Fun Bahasa landing page. Covers the custom config, build command, design tokens, and version-specific gotchas.
license: ISC
compatibility: opencode
metadata:
  project: funbahasa-landingpage
  stack: html-tailwind-alpine
---

# Tailwind CSS on Fun Bahasa Landing Page

## Version & constraints
- Tailwind CSS **v2.2.19**, installed as a local devDependency. Do **not** assume v3+ class names or config behavior.
- Static HTML pages at repo root consume the generated `css/style.css`.
- Source directives live in `styles.css`; do not hand-edit `css/style.css`.

## Build
- Do not use `npm run build`; the script uses a Windows path (`css\style.css`) and fails on macOS/Linux.
- Correct command: `npx tailwindcss build styles.css -o css/style.css`
- `purge: []` in `tailwind.config.js` means the full framework is emitted (~236k lines). Enabling purge will shrink it, but include `js/data-*.js` files in `content` to avoid dropping classes used only in JS strings.

## Custom theme tokens (`tailwind.config.js`)
- **Blue palette**: `blue-100` (#d3eef9) through `blue-900` (#07222d). Brand primary is around `blue-500` (#25aae3) / `blue-600` (#1e88b6).
- **Gray palette**: `gray-100` (#d9dad9) through `gray-900` (#0d0e0d).
- **Orange**: defined as `'FCB316'` — **missing the leading `#`**, so `bg-orange` / `text-orange` will not render as that hex. Prefer the amber/yellow scale or fix the config if orange is required.
- **Breakpoints**: extra `3xl: 1920px` and `4xl: 2304px` beyond the default Tailwind set.
- **Shadows**: prefer project-defined `shadow-soft` and `shadow-pop` over default shadows.
- **Font**: `Poppins` is the default `font-sans`.

## Design conventions
- Use `font-semibold`, not `font-bold` / `font-extrabold`, for headings, nav links, buttons, and footer labels.
- Add `transition duration-200 ease-out` to interactive elements (cards, buttons, links, tabs, pagination arrows, social icons).
- Cards lift on hover: `hover:-translate-y-1 hover:shadow-soft`.
- Card grids: wrap card content with `flex flex-col flex-grow` and place the author row with `mt-auto` for bottom alignment.
- Smooth scrolling is set globally in `styles.css`.

## v2 vs v3 watch-outs
- Variant syntax in v2 requires enabling variants explicitly. The config already enables `dark`, `group-hover`, `focus-within`, etc.
- `darkMode: false` — dark variants are generated but not auto-applied by OS preference.
- Arbitrary values (e.g. `w-[123px]`) are **not** available in v2 without JIT; use standard spacing or extend the config.
- `line-clamp-*` is not built-in; implement with custom utilities if needed.
- Avoid modern v3-only utilities such as `aspect-ratio`, `scroll-mt-`, `touch-pan-`, etc.

## Adding new utilities
- Extend existing theme sections (colors, spacing, shadows, breakpoints) in `tailwind.config.js` when possible.
- Add custom component classes inside `styles.css` using `@layer components { ... }` and `@apply`.
- Rebuild after any config or `styles.css` change.

## Gotchas
- `css/style.css` is committed to git; rebuild and commit it after theme changes.
- Generated classes used inside `js/data-*.js` string templates are invisible to purge unless the JS files are scanned.
