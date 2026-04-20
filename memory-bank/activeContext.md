---
liquid: false
---

# Active Context - Current State

_Last refreshed: 2026-04-20_

## Status

Site is **live in production** at `https://mannacafe.dk`. Initial HTML → Jekyll conversion and all Tailwind-era optimizations are in the past; the project is now in ongoing-maintenance mode.

## Current Architecture at a Glance

- **Styling**: hand-authored SCSS in `_sass/` (no Tailwind). Design tokens as CSS custom properties in `:root`. BEM-style class naming (`site-header`, `menu-page__item-name--extra`, etc.).
- **Typography**: Inter only. Single `--font-family` token, one cascade point on `html`, no per-component `font-family` declarations. Weight/italic variations handled per element with `font-weight`/`font-style`.
- **CSS pipeline**:
  1. `:root` tokens live in `_sass/_tokens.scss` and are imported by BOTH `_critical.scss` and `main.scss`, so variable values can't diverge between the inlined and async stylesheets.
  2. `_sass/_critical.scss` owns every above-the-fold rule (base reset, layout primitives, site-header/nav/logo, per-page heroes — home, menu incl. items, om-os hero — plus the prefers-reduced-motion fallback). Non-critical partials do not redeclare these selectors. This was refactored after an audit found drifting `.site-header__logo` widths and unstyled menu items during FOUC.
  3. Jekyll compiles `assets/css/main.scss` (imports `_tokens`, `_variables`, `_base`, `_layout`, `_components`, `_utilities`, `_home`, `_om-os`) on every build.
  4. Pre-commit hook runs `scripts/inline-critical-css.js` — compiles `_sass/_critical.scss` with `sass` package (loadPaths set to `_sass`) and injects compressed output between `<!-- CRITICAL_CSS_START -->` / `<!-- CRITICAL_CSS_END -->` markers in `_includes/head.html`.
  5. Production build (`npm run build`) runs PurgeCSS over the built `_site/` to strip unused selectors from `main.css`.
- **Data**: `_data/new_menu.yml` is the active menu — flat list of `sections`, each with a `title`, optional `description` (used as a time subtitle), and an `items` list. Items have `name`, `price`, optional `description`, and optional `name_extra` (renders parenthesised after the name, e.g. `(V)` / `(Veg)`). `_data/menu.yml` is legacy/unused.
- **Layout**: single `_layouts/default.html` wraps every page — head.html, header.html, `{{ content }}`, footer.html.
- **Pages**: `index.md` (fullscreen hero video + centred logo + "cafe" tagline, no header logo, no footer), `menu.md` (renders new_menu.yml as a CSS multi-column list — sections auto-distribute across two columns at `lg`+), `om-os.md` (red hero with photo-left + text-right, followed by a black full-bleed video section). Non-home pages show the small logo in the top-left of the header linking back to `/` and the 4-column footer at the bottom.
- **Logo**: `_includes/logo.html` renders a masked `<span class="site-logo">`; the single SVG lives at `assets/logo.svg` with `currentColor` fills and a viewBox tightened to just the wordmark (aspect ≈ 673/202). Colour is overridable per page via `logo_color` frontmatter → inline `--logo-color` on `<body>`. Size is driven by a sibling class (`site-header__logo`, `home__logo`, …).
- **Per-page theming**: five frontmatter fields pipe through to `<body>` as CSS custom properties — `logo_color` → `--logo-color`, `nav_bg` → `--nav-bg`, `nav_color` → `--nav-color`, `page_bg` → `--page-bg`, `footer_bg` → `--footer-bg`. Consumers use them with `var(…, fallback)`, so pages override only what they need.
- **Brand palette (2026 redesign)**: `--color-brand: #be1e2d`, `--color-brand-dark: #9f1b27`, `--color-pale: #f0fbff` alongside the original rose/blue/grey scales.
- **Global footer**: 4 columns (Visit us / Kontakt / Følg med / Åbningstider) rendered on every page *except* `/`, driven by `site.address_lines`, `site.email`, `site.social_links.*`, and `site.opening_hours` in `_config.yml`. Background defaults to the page's `--page-bg`; pages with a dark body (e.g. Om os) set `footer_bg` in frontmatter to opt into a light footer bg. Text is brand-red (`--color-brand-dark`), with a hairline divider above. **Note**: config uses `social_links` (not `social`) because `jekyll-seo-tag` mutates `site.social.*` and strips URLs down to usernames.
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
