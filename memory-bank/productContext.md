---
liquid: false
---

# Product Context - Manna Café Website

## Why This Project Exists

- Jane (family member) opened Cafe Manna and needs a professional online presence in Danish
- Frequent menu updates (seasonal ingredients, new wines, price adjustments)
- Limited technical expertise → editing must be possible without a local dev setup

## Problems It Solves

1. **Online presence**: modern, Scandinavian-aesthetic site representing the café
2. **Self-serve content management**: menu edits via GitHub.com
3. **Zero-cost hosting**: GitHub Pages free tier
4. **Low maintenance**: no CMS, no database, no backend
5. **Mobile reach**: customers check menu + location on phones

## How It Should Work

### For Customers

- Clean Danish-language site with café story, menu, and practical info
- Multi-level menu (morgenmad, frokost, tapas, brunch, desserts, drinks, wine, cocktails)
- Address linked to Google Maps, Instagram link in footer
- Large hero image on homepage, photo of Jane on "Om os"

### For the Content Editor (Jane / family)

- Navigate to `_data/new_menu.yml` on github.com/kasperfilstrup/mannacafe
- Click pencil icon → edit YAML → commit changes
- GitHub Action rebuilds and publishes to `mannacafe.dk` within a few minutes

### Menu YAML structure (3 nesting levels supported)

```yaml
sections:
  - title: Section name            # e.g. "Morgenmad"
    description: Optional          # italic note under title
    items:                         # leaf items
      - name: Dish name
        name_extra: Optional       # renders in parentheses, smaller font
        description: Optional
        price: "45 kr"
    subcategories:                 # OR nested groups (e.g. "Tapasbræt", "Tapas")
      - title: Subcategory name
        description: Optional
        items: [...]
        subcategories:             # third level (e.g. Drikkevarer → Øl → Fadøl)
          - title: ...
            items: [...]
```

### For the Developer

- Edit SCSS partials in `_sass/` for styling
- Edit `_includes/head.html|header.html|footer.html` for chrome
- Edit markdown files (`index.md`, `menu.md`, `om-os.md`) for page content
- Pre-commit hook re-inlines critical CSS automatically when `_sass/*.scss` changes

## User Experience Goals

- **Simplicity**: fast to find address, menu, story
- **Beauty**: maintained Playfair Display + rose/blue palette
- **Speed**: inlined critical CSS + async main stylesheet + `font-display: optional`
- **Accessibility**: semantic HTML, mobile-first responsive
- **Trust**: professional Danish copy, real photos, real address/social links
