---
liquid: false
---

# Maintenance & Change Workflow

> Note: This file used to describe the one-time HTML → Jekyll conversion. That conversion is done. This file now documents how to make day-to-day changes safely.

## When making changes

### 1. Menu content (most common)
- Edit `_data/new_menu.yml`
- Structure: `sections[] → items[] | subcategories[] → items[] | subcategories[] → items[]`
- Per item: `name`, optional `name_extra` (renders in parens), optional `description`, `price` (string, e.g. `"45 kr"` or `"80/315 kr"` for glass/bottle)
- Preview locally with `npm run dev`
- Commit + push → live in ~1–2 min via GitHub Actions

### 2. Copy / text on pages
- Home: `index.md`
- About/contact: `om-os.md`
- Menu heading copy: `menu.md` frontmatter (title/description) — rest pulls from data

### 3. Site-wide info (address, nav, social, email)
- `_config.yml`
- Referenced via `site.title`, `site.address`, `site.email`, `site.social.instagram`, `site.navigation`, etc.

### 4. Styling
- Edit SCSS partials in `_sass/`
- If the change touches **above-the-fold** styles (header, hero, first section, menu page top) also update `_sass/_critical.scss`
- Pre-commit hook will auto-compile and re-inline `_critical.scss` into `_includes/head.html`. Do not hand-edit the block between `CRITICAL_CSS_START`/`END` markers
- For brand colours / spacing / typography changes → `_sass/_variables.scss` (tokens are CSS custom properties)

### 5. Chrome (header/footer/meta)
- `_includes/head.html` — meta, fonts, critical CSS block, stylesheet preload
- `_includes/header.html` — nav
- `_includes/footer.html` — social links + copyright

### 6. Adding a new page
- Create `new-page.md` in repo root with frontmatter `layout: default`, `title: ...`, `description: ...`
- Add nav entry under `site.navigation` in `_config.yml`
- Use existing `.page-padding`, `.page-section`, or `.contact-page` classes; avoid new one-off styles when an existing component works

## Pre-flight before committing

- `npm run dev` and visually check pages affected
- For menu changes: check deepest nesting you touched still renders (e.g. Drikkevarer → Øl → Fadøl)
- For SCSS changes: confirm mobile and ≥md breakpoints

## Deploy flow
1. Push to `main`
2. `.github/workflows/jekyll.yml` runs `jekyll build` with `_config.yml,_config.production.yml`
3. `actions/deploy-pages` publishes
4. `mannacafe.dk` serves updated site (CDN propagation can take a moment)

## When things go wrong

- **Build fails on Actions**: check the workflow run logs. Usually a YAML syntax error in `_data/new_menu.yml` or a Liquid typo in a page.
- **Menu item missing**: YAML indentation — items must be under `items:` or `subcategories:`, not floating.
- **Styling didn't update**: verify that for above-the-fold changes you edited `_critical.scss` too, not only `_components.scss` etc.
- **Critical CSS block looks corrupted**: run `npm run inline-critical-css` manually and commit the result.
