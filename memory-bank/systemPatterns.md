---
liquid: false
---

# System Patterns - Architecture

## Core Pattern

**Jekyll + hand-authored SCSS + data-driven content.** Component-based includes, single layout, design tokens via CSS custom properties, production build post-processes CSS with PurgeCSS, critical CSS is inlined into `<head>` via a pre-commit hook.

## File Organization

```
/
├── _config.yml                 # site-wide config (nav, social, address)
├── _config.production.yml      # prod-only overrides (sass: compressed)
├── _data/
│   ├── new_menu.yml            # ACTIVE menu data (nested)
│   └── menu.yml                # legacy, unused
├── _includes/
│   ├── head.html               # meta, fonts, inlined critical CSS, deferred main.css
│   ├── header.html             # logo (non-home) + nav
│   ├── footer.html             # social + copyright
│   ├── logo.html               # reusable MANNA mark (CSS mask consuming /assets/logo.svg)
│   └── banner.html             # exists but not currently included
├── _layouts/
│   └── default.html            # single layout
├── _sass/
│   ├── _variables.scss         # design tokens (custom props + SASS $vars + mixins)
│   ├── _base.scss              # html/body/headings/anchors resets
│   ├── _layout.scss            # page-wrapper, container, grid
│   ├── _components.scss        # site-logo, site-header, site-nav, site-footer, cafe-btn, cafe-card
│   ├── _utilities.scss         # page-section, feature-card, contact-page, menu-page
│   ├── _home.scss              # home-page progressive-enhancement rules (reduced-motion fallback)
│   └── _critical.scss          # above-the-fold styles (inlined into head.html)
├── assets/
│   ├── css/main.scss           # Jekyll-compiled entry: @import all partials
│   ├── logo.svg                # MANNA wordmark — fills are currentColor so CSS controls recolouring
│   ├── icon.svg                # unrelated monogram M (favicon uses an inline data-URI variant)
│   ├── images/                 # cafe-manna.webp, cafe-manna-kort.webp, jane.jpg
│   └── video/manna-temp.mp4    # home-page hero background video
├── scripts/
│   └── inline-critical-css.js  # sass compile of _critical.scss → injected into head.html
├── index.md                    # hero + CTA
├── menu.md                     # renders new_menu.yml (3-level loop)
├── om-os.md                    # about + contact + map
├── Gemfile                     # github-pages gem
├── package.json                # dev tooling (sass, purgecss, husky, lint-staged)
└── .github/workflows/jekyll.yml
```

## Design Patterns

### Design Tokens as CSS Custom Properties
`:root { --primary-500: #7e3547; --spacing-8: 2rem; --font-family: var(--font-noto); ... }`
Partials consume tokens, never raw hex values. Breakpoints are SASS variables (`$breakpoint-md: 768px`) used through a `respond-to` mixin.

### BEM Class Naming
`.menu-page`, `.menu-page__section`, `.menu-page__item-name`, `.menu-page__item-name--extra`. Matches the compile-to-CSS mental model and stays readable in the generated HTML.

### Data-Driven Menu Rendering
`menu.md` loops over `site.data.new_menu.sections`, then optionally iterates `section.items`, `section.subcategories`, and inside each subcategory the same (items, third-level subcategories). Empty `description` and missing `name_extra` are handled with `!= '' and != nil` guards.

### Config-Driven Chrome
- `site.navigation` → header nav links
- `site.social.instagram` / `site.social.facebook` → footer icons
- `site.address` → linked to Google Maps on `om-os.md`
- `site.title` / `site.description` → page `<title>` and meta description fallbacks

### Reusable Logo (CSS mask pattern)
- Asset: `assets/logo.svg` — every path's `fill` is `currentColor`
- Include: `{% include logo.html class="..." %}` → renders `<span class="site-logo ...">` (semantic via `role="img"` + `aria-label={{ site.title }}`)
- Styling (in `_critical.scss` so it paints instantly):
  ```scss
  .site-logo {
    display: inline-block;
    aspect-ratio: 1 / 1;
    background-color: currentColor;
    mask: url('/assets/logo.svg') center / contain no-repeat;
  }
  ```
- **Colour per page**: set `logo_color: '#be1e2d'` in page frontmatter. `_layouts/default.html` writes that to the `<body>` as `style="--logo-color: …"`. The logo's parent uses `color: var(--logo-color, var(--color-brand))`, which cascades into `currentColor`.
- **Size per context**: sibling class sets width only. `.site-header__logo` → 3–3.5rem; `.home__logo` → `min(70vmin, 500px)`.
- **Home vs. other pages**: `_includes/header.html` checks `page.url == '/'` — logo is omitted from the header on home (the hero section has a large centred copy), and the header uses `.site-header--overlay` to float the nav pill over the video.

### Home-page hero (video background)
- `index.md` contains `<section class="home">` with a fullscreen `<video autoplay muted loop playsinline>` at `z-index: -1` behind `.home__content` (logo + tagline).
- Section sized `min-height: 100dvh` (fallback `100vh`), `object-fit: cover` on the video.
- `@media (prefers-reduced-motion: reduce)` hides the video and falls back to `--color-pale` background.
- Header `.site-header--overlay` is `position: absolute; top: 0; right: 0; z-index: 2` so the red nav pill sits over the top-right corner of the video.

### Critical CSS Inlining (via pre-commit)
1. Author writes styles in `_sass/_critical.scss` (above-the-fold only)
2. On commit, husky runs `lint-staged`, which runs `scripts/inline-critical-css.js` when any `_sass/**/*.scss` is staged
3. Script compiles critical SCSS compressed, replaces content between `CRITICAL_CSS_START`/`END` markers in `_includes/head.html`, re-stages `head.html`
4. Result: first paint needs no external CSS fetch; main stylesheet loads async via `rel=preload + onload`

### Production CSS Purging
`npm run build` builds Jekyll with production config (compressed SASS), then runs `purgecss --css _site/assets/css/main.css --content '_site/**/*.html'` to strip unused selectors.

⚠️ GitHub Actions deploy currently does NOT run PurgeCSS — it only runs `jekyll build`. If bundle size matters in production, add a purge step to the workflow.

## Component Communication

- Layout includes header/footer via `{% include %}`
- Pages specify `layout: default` in frontmatter → wrapped by `_layouts/default.html`
- Active nav link: `{% if page.url == item.url %}header-nav__link--active{% endif %}`
- Page title: `{% if page.title %}{{ page.title }} - {{ site.title }}{% else %}{{ site.title }}{% endif %}`

## Fonts & Typography

- Single family: **Inter** (Google Fonts variable font, full weight range 100–900, upright + italic, `font-display: optional`)
- Single token: `--font-family: 'Inter', sans-serif;` in both `_variables.scss` and `_critical.scss`
- Single cascade point: `html { font-family: var(--font-family); }` — no other `font-family` declarations in partials. Everything inherits. Weight/style variations are applied per element via `font-weight` / `font-style` only.

## Responsive Strategy

Mobile-first. Breakpoints: xs 480, sm 640, md 768, lg 1024, xl 1280. Most layout switches happen at `md`. Hero image uses an extreme off-canvas crop on mobile then centers at ≥480px.
