# GHOSTTRACE — website

Static marketing + legal site for [GHOSTTRACE](https://github.com/ghosttrace/ghosttrace), an open-source,
self-hosted personal activity-monitoring / OSINT dashboard.

Plain HTML/CSS — no build step, no dependencies, no JavaScript required to read the site.
It can be served from any static host.

**Live URL (canonical):** `https://parasbutt3-dotcom.github.io/ghosttrace-site/`
This base URL is baked into `canonical`/`og` tags, `robots.txt` and `sitemap.xml`.
Moving to a custom domain or Vercel? Search `parasbutt3-dotcom.github.io/ghosttrace-site`
across the repo (5 files: 3 HTML + robots.txt + sitemap.xml) and replace with the new base.

## Files

```
index.html      Landing page (hero, features, privacy, quick start)
privacy.html    Privacy policy — states exactly what the software collects
terms.html      Terms of service — MIT, authorized use, operator responsibility
style.css       Shared design system (dark SOC theme, tokens in :root)
favicon.svg     Ghost-in-crosshair mark, for the tab
logo.svg        Same mark as the app logo (512×512 viewBox)
logo.png        512×512 raster export (Google consent screen logo; ≤ 2 MB)
robots.txt      Crawler rules + sitemap pointer
sitemap.xml     The three pages
.nojekyll       Tells GitHub Pages to serve files verbatim
README.md       This file
```

## Deploy on GitHub Pages

Already set up for this repo (`ghosttrace-site` on the `main` branch):

1. **Settings → Pages → Source: Deploy from a branch**, branch `main`, folder `/ (root)`.
   (CLI equivalent: `gh api -X POST repos/<owner>/ghosttrace-site/pages -f build_type=branch -f 'source[branch]=main' -f 'source[path]=/'`)
2. The site appears at `https://<owner>.github.io/ghosttrace-site/` within a minute.
3. Custom domain (optional): add a `CNAME` file, set the domain in Pages settings,
   point your DNS at GitHub, then update the base URL as described above.

## Deploy on Vercel

Zero config — there is nothing to build.

**Dashboard**

1. Push this directory to a Git repository.
2. In Vercel: **Add New → Project → Import** the repo.
3. Framework preset: **Other**. Leave *Build command* empty and *Output directory* as `./`.
4. Deploy, then attach your domain and update the base URL as described above.

**CLI**

```bash
npx vercel        # first run links/creates the project
npx vercel --prod
```

## Before you go live — placeholders to replace

- `https://github.com/ghosttrace/ghosttrace` — the **product** repo link used on all three
  pages. That repository is the software itself and doesn't exist yet; when it does,
  replace the link (or keep pointing at it once published).
- The quick-start commands on `index.html` are illustrative; align them with the real
  repository's compose setup before publishing.
- `logo.png` corners are transparent — re-render with a full-bleed dark square if your
  upload target rejects transparency (Google's OAuth consent screen accepts PNG ≤ 2 MB;
  the current file is far under that).

## Re-exporting logo.png

`logo.svg` is the source of truth. Any SVG rasterizer works; examples:

```bash
rsvg-convert -w 512 -h 512 logo.svg -o logo.png
# or: inkscape logo.svg --export-type=png --export-width=512 --export-filename=logo.png
```

## License

The site content and assets are MIT-licensed, same as the software:
see the [repository LICENSE](https://github.com/ghosttrace/ghosttrace/blob/main/LICENSE).
