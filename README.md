# VH Legend — product website

Static single-page site. No build step, no dependencies.

## Deploy to Vercel

**Option A — drag and drop**
1. Go to vercel.com/new
2. Drag this whole folder onto the page
3. Framework preset: **Other**. Leave build command and output directory empty.
4. Deploy.

**Option B — CLI**
```bash
npm i -g vercel      # once
cd vhlegend-site
vercel               # preview URL
vercel --prod        # production
```

**Option C — Git**
Push the folder to a GitHub repo, then Import Project in Vercel.
Framework preset: Other. No build command.

## Custom domain
Vercel dashboard → Project → Settings → Domains → add `vhlegend.com`
(or `www.vhlegend.com`), then point the DNS records Vercel shows you.

## Files
- `index.html` — the whole page (HTML, CSS, JS inline)
- `assets/` — images pulled from the capabilities deck
- `vercel.json` — caches `/assets/*` for a year

## Editing
All copy is in `index.html`. The 8 rack configurations live in the
`data` object in the `<script>` block at the bottom — edit there to
change the interactive grid.
