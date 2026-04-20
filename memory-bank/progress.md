---
liquid: false
---

# Progress - Manna Café

## ✅ Current State (2026-04-20)

- **Live at** `https://mannacafe.dk` via GitHub Pages + CNAME
- **Language**: Danish throughout
- **Design**: Playfair Display, pink-rose primary + dusty-blue secondary palette
- **Pages**: `/` (hero + CTA), `/menu/`, `/om-os/`
- **Menu source**: `_data/new_menu.yml` with 3 nesting levels
- **Deploy**: automated via `.github/workflows/jekyll.yml` on push to `main`

## ✅ Implemented

### Site Infrastructure
- Single `_layouts/default.html` + includes for head/header/footer
- Navigation generated from `_config.yml` → Hjem / Menu / Om os
- Active-nav highlighting via `page.url` comparison
- Clean pretty permalinks

### Content
- **Home** (`index.md`): hero image, CTA button linking to menu
- **Menu** (`menu.md`): data-driven renderer that handles section → subcategory → sub-subcategory, optional `name_extra` parenthetical, optional descriptions, price field
- **About** (`om-os.md`): Jane's story, address (linked to Google Maps), phone/email, social icons, location map image (with mobile/desktop `<picture>` source)

### Styling
- SCSS partials with design tokens as CSS custom properties
- BEM-style class naming
- Responsive mixin (`@include respond-to('md') { ... }`)
- Component classes: `.site-header`, `.site-footer`, `.cafe-btn`, `.cafe-card`, `.feature-card`, `.menu-page__*`, `.contact-page__*`

### Performance
- Critical CSS compiled from `_sass/_critical.scss` and inlined into `_includes/head.html` via `scripts/inline-critical-css.js` (triggered by husky pre-commit when any `_sass/*.scss` file is staged)
- Main stylesheet loaded async
- PurgeCSS runs on `npm run build` (local only)
- Font preloaded + `font-display: optional`
- Inline SVG favicon
- Hero image: WebP, `fetchpriority="high"`, explicit dimensions

### Deploy
- GitHub Actions workflow: `actions/checkout` → `ruby/setup-ruby` (3.1, bundler-cache) → `actions/configure-pages` → `jekyll build` with prod config → `actions/upload-pages-artifact` → `actions/deploy-pages`
- No Jekyll plugins beyond what `github-pages` gem allows — but using Actions means we're not limited by the whitelist

## 📝 Recent Notable Changes (git log)

- `f2c6828` Remove 'opening soon' banner
- `24769d6` / `72e5c55` Instagram link/handle updates
- `1700932` Menu item description + styling consistency tweaks
- `eea4f68` Add `name_extra` field for menu items + display logic

## 🧹 Known Cruft / Debt

- `src/input.css` — legacy Tailwind source, safe to delete
- `DEVELOPMENT.md` — describes Tailwind workflow, rewrite or delete
- `README.md` — mentions Tailwind + old English menu structure, needs refresh
- `_data/menu.yml` — old English placeholder data, unused, safe to delete
- `_includes/banner.html` — no longer included in layout (removed by f2c6828); delete file or keep for future reuse
- `_config.yml` lists `tailwind.config.js` / `postcss.config.js` in `exclude` but those files no longer exist (harmless)
- `_config.yml` has `url: https://kasperfilstrup.github.io` — worth verifying this doesn't leak into any generated URL on the live domain
- `assets/video/manna-temp.mp4` — untracked video file sitting in working tree
- GitHub Actions deploy does NOT run PurgeCSS — only local `npm run build` does. If we care about shipped CSS size on the live site, add a purge step to the workflow

## 🔮 Future Enhancements (not scheduled)

- Add hero/section photography for home page beyond single image
- Consider blog/events collection for announcements
- Add structured data (JSON-LD) for local business SEO
- Optimize the map image further (static Google Maps API or vector SVG)
- Consider internationalization if English customers ever become a need
