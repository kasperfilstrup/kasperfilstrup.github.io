---
liquid: false
---

# Active Context - Current State

_Last refreshed: 2026-04-20_

## Status

Site is **live in production** at `https://mannacafe.dk`. Initial HTML → Jekyll conversion and all Tailwind-era optimizations are in the past; the project is now in ongoing-maintenance mode.

## Current Architecture at a Glance

- **Styling**: hand-authored SCSS in `_sass/` (no Tailwind). Design tokens as CSS custom properties in `:root`. BEM-style class naming (`site-header`, `menu-page__item-name--extra`, etc.).
- **CSS pipeline**:
  1. Jekyll compiles `assets/css/main.scss` (imports `_variables`, `_base`, `_layout`, `_components`, `_utilities`) on every build
  2. Pre-commit hook runs `scripts/inline-critical-css.js` — compiles `_sass/_critical.scss` with `sass` package and injects compressed output between `<!-- CRITICAL_CSS_START -->` / `<!-- CRITICAL_CSS_END -->` markers in `_includes/head.html`
  3. Production build (`npm run build`) runs PurgeCSS over the built `_site/` to strip unused selectors from `main.css`
- **Data**: `_data/new_menu.yml` is the active menu (multi-level). `_data/menu.yml` is legacy/unused.
- **Layout**: single `_layouts/default.html` wraps every page — head.html, header.html, `{{ content }}`, footer.html.
- **Pages**: `index.md` (hero + CTA), `menu.md` (renders new_menu.yml with 3-level nesting), `om-os.md` (about + location/social).
- **Deploy**: `.github/workflows/jekyll.yml` runs `JEKYLL_ENV=production bundle exec jekyll build --config _config.yml,_config.production.yml` on push to `main`, then publishes via `actions/deploy-pages`. (No PurgeCSS step in CI — only in local `npm run build`.)

## Known Cruft / Follow-ups

- `src/input.css` — leftover Tailwind entry file, unused
- `DEVELOPMENT.md` — describes old Tailwind workflow, outdated
- `README.md` — Tailwind + English menu references, outdated
- `_data/menu.yml` — old English placeholder, unused
- `_includes/banner.html` — exists but no longer included in `default.html` (removed by commit f2c6828)
- `_config.yml` still has `url: https://kasperfilstrup.github.io` and `baseurl: ""` — domain override may come from `_config.production.yml` or GH Pages itself; worth verifying if URLs ever look wrong
- `assets/video/` — untracked `manna-temp.mp4` on disk

## Active Decisions

- Keep SCSS custom-authored — no plan to reintroduce Tailwind
- Keep critical CSS inlining via pre-commit hook (trade-off: `_includes/head.html` is auto-mutated, but first-paint stays fast)
- Keep menu in YAML (not Markdown) — easier for Jane to edit structured data
- Single `default` layout is sufficient; no per-page layouts needed
