# VH Legend — brand website

Static single-page site. No build step, no dependencies to install.

```
index.html                  the page
assets/                     images referenced by index.html
VHLEGEND-DESIGN-SYSTEM.md   tokens, component rules, accessibility criteria, QA checklist
```

## Run locally
Open `index.html` in a browser, or serve the folder:
`python3 -m http.server 8000`

## Deploy (GitHub Pages)
Settings → Pages → Branch: `main`, folder: `/` (root).

## Notes
- Mona Sans loads from Google Fonts; the fallback stack is Helvetica/Arial.
- Phones and small tablets open in the desktop layout by default
  (`<meta name="viewport" content="width=1280">`). The responsive layout is
  built and reachable from the on-page switcher or the `#mobile` deep link.
  See section 3.6 of the design system before changing this.
- Replacing an image: keep the filename, or update the `src` in `index.html`.
