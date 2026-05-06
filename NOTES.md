# Homepage — handoff notes

Picking up on another device? Start here.

## What's in this repo

Imported from the Claude Design bundle `crewdriver-design-system` (handoff
from claude.ai/design):

- `variants/index.html` — gallery page linking to 10 homepage concepts
- `variants/v1.html` … `v10.html` — the 10 standalone concept pages
- `variants/thumbs/` — gallery thumbnails (jpg)
- `colors_and_type.css` — shared color tokens + ChunkFive `@font-face`
- `fonts/ChunkFive-Regular.{woff2,woff,ttf}` — display font
- `assets/icon.png` — favicon

The variant pages are self-contained (CSS inline). `variants/index.html` is
the only file that imports `../colors_and_type.css` and the favicon.

Open `variants/index.html` in a browser to see the gallery.

## Hosting decision

**Ship on GitHub Pages now. Port to Astro later if duplication forces it.**

Why:
- Files are already plain HTML, no build step.
- GitHub Pages is zero-config for this layout: enable in repo Settings →
  Pages, point at this branch (or `main` once merged), and
  `https://jcharmer.github.io/homepage/variants/` is live.
- Cloudflare Pages is the alternative if you want a faster CDN / easier
  custom domain — same "deploy from repo" flow, no build command.

When to switch to Astro:
- Once you start duplicating headers/footers/nav across the 10 pages.
- Once you want content collections, MDX, or a real component model.
- Astro deploys cleanly to GitHub Pages, Cloudflare Pages, Netlify, Vercel.

Other frameworks (Next, Hugo, Jekyll, 11ty, Gatsby, Nuxt, SvelteKit,
Docusaurus, MkDocs, VitePress) are fine but overkill for the current state.

## Deploy on GitHub Pages (when ready)

1. Merge this branch to `main` (or set Pages source to this branch).
2. Repo Settings → Pages → Source: deploy from branch → `main` / `/ (root)`.
3. Wait ~1 min, visit `https://jcharmer.github.io/homepage/variants/`.
4. Custom domain: add a `CNAME` file at repo root with the domain, then
   set the DNS records GitHub shows you.

## Next steps / open questions

- Pick a landing variant (or keep the gallery as `/` for now).
- Decide on custom domain.
- If/when porting to Astro: one `Layout.astro` + one `[variant].astro`
  page using the existing v1–v10 markup as content.

## Branch

Working branch: `claude/add-local-files-G5ZNM`. No `main` branch exists
yet — first push created the repo's initial history here.
