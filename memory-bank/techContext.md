---
liquid: false
---

# Technical Context

## Stack

- **Static site generator**: Jekyll (GitHub Pages gem)
- **Hosting**: GitHub Pages with custom domain `mannacafe.dk` (CNAME file)
- **Styling**: hand-authored SCSS compiled by Jekyll's built-in Sass converter; PurgeCSS for production
- **Critical CSS**: inlined via Node script (`sass` npm package) + husky pre-commit hook
- **Fonts**: Inter from Google Fonts (variable font, weight range 100–900 + italic, async CSS via `rel=preload`/`onload`)
- **Version control / CI**: Git, GitHub Actions (`.github/workflows/jekyll.yml`)

## Dependencies

### Ruby (Gemfile)
```ruby
gem "github-pages", group: :jekyll_plugins
```
No pinned version — GitHub Pages resolves it. `Gemfile.lock` is committed.

### Node (package.json, devDependencies only)
- `sass` — compile `_critical.scss` for inlining
- `purgecss` — strip unused CSS selectors in production
- `husky` — git hooks
- `lint-staged` — run hooks only on staged files

### NPM Scripts
- `npm run dev` → `bundle exec jekyll serve --livereload`
- `npm run build` → `JEKYLL_ENV=production jekyll build --config _config.yml,_config.production.yml && npm run purge`
- `npm run purge` → PurgeCSS on built `_site/assets/css/main.css`
- `npm run inline-critical-css` → re-run the critical CSS inliner manually
- `npm run prepare` → husky install

### Husky / lint-staged
`.husky/pre-commit` runs `npx lint-staged`. Config in package.json:
```json
"lint-staged": {
  "_sass/**/*.scss": [
    "npm run inline-critical-css",
    "git add _includes/head.html"
  ]
}
```

## Configuration Files

- `_config.yml` — site config (title, description, address, email, social, navigation, markdown/sass/permalink)
- `_config.production.yml` — overrides for prod (`sass.style: compressed`)
- `Gemfile` — `github-pages` plugin group
- `package.json` — dev tooling + lint-staged config
- `.prettierrc` — formatting
- `.gitignore` — standard Jekyll (`_site/`, `.jekyll-cache/`), `node_modules/`

## Jekyll Configuration Highlights

```yaml
markdown: kramdown
sass:
  sass_dir: _sass
  style: expanded    # compressed in prod via _config.production.yml
permalink: pretty    # clean URLs like /menu/ instead of /menu.html
exclude:
  - Gemfile, Gemfile.lock, README.md, memory-bank/, node_modules/,
    package.json, package-lock.json, tailwind.config.js (legacy), postcss.config.js (legacy),
    src/, vendor/, .bundle/, _config.production.yml
```

Note the `tailwind.config.js` / `postcss.config.js` entries in `exclude` — these files no longer exist on disk but the exclude rules are harmless.

## Local Development

```bash
bundle install
npm install
npm run dev            # jekyll serve --livereload at http://localhost:4000
```

For a production-like build locally:
```bash
npm run build          # runs Jekyll + PurgeCSS
```

## Deployment

- Push to `main` → `.github/workflows/jekyll.yml` triggers
- Build step: `JEKYLL_ENV=production bundle exec jekyll build --config _config.yml,_config.production.yml`
- Publish step: `actions/deploy-pages@v4`
- `CNAME` file in repo root (`mannacafe.dk`) tells GitHub Pages which domain to serve

## Performance Tactics Currently in Place

- Critical CSS inlined in `<head>` (generated from `_sass/_critical.scss`)
- Main stylesheet deferred via `rel=preload` + `onload=this.rel='stylesheet'`
- Google Fonts: `dns-prefetch`, `preconnect`, async stylesheet via `rel=preload` + `onload`, `font-display: optional` (no specific woff2 preload — variable font URL isn't stable enough to hardcode)
- Favicon as inline data URI SVG (no extra request)
- Images as WebP with explicit width/height to avoid layout shift
- `fetchpriority="high"` on hero image

## Browser Support

Modern evergreen browsers. Mobile-first responsive. No JS required for core layout (small amount of JS for the async stylesheet swap pattern).
