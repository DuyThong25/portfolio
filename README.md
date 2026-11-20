# Đặng Duy Thông — Portfolio

Personal portfolio of a Backend .NET Developer. A single static page with Vietnamese/English content and light/dark themes. No build step, no dependencies.

## Structure

```
portfolio/
├── index.html      # markup, styles and script (the Portfolio object)
└── i18n/
    ├── vi.json     # Vietnamese strings
    └── en.json     # English strings
```

## Run locally

Translations are loaded with `fetch()`, which browsers block on `file://`. Opening `index.html` by double-click shows keys such as `[Vi] HERO_LEAD` instead of text. Serve the folder over HTTP instead:

```bash
npx --yes serve .
```

Then open http://localhost:3000. Any static server works, for example `python -m http.server 8000` or the VS Code Live Server extension.

## Editing content

All visible text lives in `i18n/vi.json` and `i18n/en.json`. The HTML only holds keys:

```html
<p class="lead" data-i18n="HERO_LEAD">HERO_LEAD</p>
<nav data-i18n-aria="NAV_LABEL" aria-label="NAV_LABEL">
```

- `data-i18n="KEY"` replaces the element's text.
- `data-i18n-aria="KEY"` replaces its `aria-label`.
- Keys are `UPPER_SNAKE_CASE`, prefixed by section: `NAV_`, `HERO_`, `ABOUT_`, `SKILLS_`, `EXP_`, `EDU_`, `PROJ_`, `CONTACT_`.

To add or change text:

1. Add the key to the HTML element.
2. Add the same key to **both** `vi.json` and `en.json`.
3. Reload the page and check the browser console.

A key missing from the current language renders as `[Eng] KEY` or `[Vi] KEY`, and the console warns `[i18n] Missing keys in i18n/<lang>.json` with the full list. If a file cannot be loaded, it logs `[i18n] Failed to load i18n/<lang>.json`. JSON does not allow comments or trailing commas; a syntax error makes every string fall back to its key.

Values are inserted with `textContent`, so HTML inside a value is shown as plain text.

Names, dates, tech chips and contact links are written directly in `index.html` because they are the same in both languages.

## Behaviour

- **Language**: the saved choice is used if it is `vi` or `en`; any other saved value falls back to `vi`. With nothing saved, the browser language decides (Vietnamese browsers get `vi`, everyone else `en`).
- **Theme**: follows the system setting until the visitor toggles it; the choice is then saved.
- Both choices are stored in `localStorage` and the page still works when storage is blocked.
