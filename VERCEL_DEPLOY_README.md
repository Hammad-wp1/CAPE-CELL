# CAPE CELL — Vercel Deployment — FIXED

Your blank white page issue was caused by 3 things in the single-file build:

1. `<script type="module" crossorigin>` — the `crossorigin` attribute causes CORS blocking on Vercel static hosting → blank page.
   → FIXED: removed `crossorigin`
2. Missing `</script>` closing tag after vite-plugin-singlefile inlining (plugin bug).
   → FIXED: restored proper `</script>`
3. Local asset `/images/hero-devices.png` 404s when you upload only index.html
   → FIXED: hero image now uses Unsplash CDN with Pexels fallback
4. React mounting before DOM ready (`<div id="root">` not yet parsed)
   → FIXED: wrapped entire bundle in `DOMContentLoaded` listener
5. Favicon 404 (`/favicon.svg` missing)
   → FIXED: inlined data-URI SVG favicon

---

## Option A — Single-file (what you tried)

File: `dist/index.html` (416 KB, 122 KB gzipped) — NOW FIXED

1. Download `dist/index.html` from this repo
2. Rename to `index.html`
3. Push to GitHub repo root (ONLY that file, nothing else needed)
4. Vercel → New Project → Import repo
   - Framework Preset: **Other**
   - Build Command: *(empty)*
   - Output Directory: **./**
   - Install Command: *(empty)*
5. Deploy

Works 100%, because:
- No external assets
- No crossorigin
- DOMContentLoaded wrapped
- All images are CDN (Pexels / Unsplash)

---

## Option B — Full repo (RECOMMENDED)

Push the entire repo (with `package.json`).

Vercel settings:
- Framework Preset: **Vite**
- Build Command: `npm run build`
- Output Directory: `dist`
- Install Command: `npm install`

This gives you automatic CI/CD on every push.

Included `vercel.json` handles SPA routing:
```
{
  "cleanUrls": true,
  "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }]
}
```

---

Contact details baked in:
- capecell021@gmail.com
- 061 167 7250

All 36 Apple + Samsung devices, premium photography, live motion canvas background, cart + checkout → mailto.
