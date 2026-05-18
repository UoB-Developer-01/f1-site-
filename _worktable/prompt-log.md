
# Add readme

```
add read me  for repo based on project
```

`README.md` created. It covers:

- **Driver table** with all 10 featured racers
- **Features** — animations, responsive layout, fonts, scroll reveal
- **Project structure** — file tree
- **Usage** — open directly or via a local server
- **Data schema** — how to add/edit drivers in the JS array
- **License note** referencing Wikimedia Commons image sources


# Adding documention

```
please add a technical documentation markdown file based on @index.html
```

Created `TECHNICAL_DOCS.md` covering:

- **Architecture** — single-file structure, data-driven rendering pattern
- **CSS Design System** — all custom properties, typography choices, layout grid breakdown
- **JavaScript** — data model schema, `buildCard` logic, stat bar calculation references, speed line generation, and scroll reveal implementation
- **Responsive breakpoints** — all three media query changes documented in a table
- **Image handling** — lazy loading and error fallback
- **Animations** — all four animation mechanisms in one reference table
- **Extension guide** — how to add drivers, update stat references, or add new stat columns

# Support arabic

```
to @index.html make it in arabic and english
```

Done. The page now has full English/Arabic bilingual support:

- **Language toggle button** in the nav (top-right) switches between EN ↔ عربي
- **RTL layout** activates automatically for Arabic — card borders flip sides, stat bars fill right-to-left, corner decorations mirror, hover direction reverses
- **Cairo Arabic font** loaded alongside Barlow Condensed/Titillium Web
- All text translated: nav, hero, section header, card stats labels, rank label, championship badge, footer
- Driver names, nationalities, years, and bios all have Arabic versions
- Page `<title>` and `lang`/`dir` attributes on `<html>` update on toggle