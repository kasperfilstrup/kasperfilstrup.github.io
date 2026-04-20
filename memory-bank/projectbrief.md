---
liquid: false
---

# Manna Café Website - Project Brief

## Project Overview

Live marketing website for Manna Café (Under Elmene 13, 2300 København S). Jekyll static site served from GitHub Pages on the custom domain `mannacafe.dk`. Language is Danish.

## Core Requirements

- **Simple business-card website** for a family-run café
- **Easy menu updates** — non-technical family member edits YAML directly on GitHub.com
- **GitHub Pages hosting** with automated deploy via GitHub Actions
- **Nested menu data** supporting sections, subcategories, and sub-subcategories (e.g. Drikkevarer → Øl → Fadøl)
- **Static site generation** — no backend
- **Custom domain** with HTTPS (`mannacafe.dk`)

## User Context

- Site owner / developer: Kasper Filstrup (experienced with React/Next.js/TypeScript)
- Content editor: family member Jane — edits menu YAML via GitHub.com
- Design aesthetic: Scandinavian/warm, pink-rose + dusty-blue palette, Playfair Display typography

## Technical Goals

1. Custom SCSS architecture with design tokens (no Tailwind)
2. Data-driven menu rendered from `_data/new_menu.yml`
3. Fast first paint via inlined critical CSS + PurgeCSS in production
4. Zero-friction deploy on push to `main`
5. Responsive behaviour maintained across breakpoints

## Success Criteria

- Family member can edit `_data/new_menu.yml` on GitHub.com without breaking the site
- Push to `main` → GitHub Action builds and publishes within a few minutes
- Site loads fast (critical CSS inline, deferred main stylesheet, optimized fonts)
- Design and responsive behaviour consistent across pages
