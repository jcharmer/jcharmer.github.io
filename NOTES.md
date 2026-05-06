# Homepage — handoff notes

Picking up on another device? Start here.

## What's in this repo

Astro static site (deployed to GitHub Pages) wrapping the design bundle
imported from claude.ai/design (`crewdriver-design-system`).

```
src/
  layouts/Layout.astro       shared <head>, favicon, font tokens
  pages/index.astro          gallery (the only Astro page so far)
public/
  v1/index.html … v10/index.html   the 10 self-contained variant pages
  thumbs/                    gallery thumbnails
  colors_and_type.css        shared color tokens + ChunkFive @font-face
  fonts/ChunkFive-Regular.*  display font (woff2/woff/ttf)
  assets/icon.png            favicon
astro.config.mjs             site + base configured for jcharmer.github.io/homepage/
.github/workflows/deploy.yml GitHub Pages deploy (Astro action)
```

The variant pages (v1–v10) are still raw HTML with inline CSS — they were
self-contained, so they live in `public/` and Astro serves them as-is at
`/v1/`, `/v2/`, etc. Convert them to `.astro` later when you want to share
chrome between them.

## Local development

```sh
npm install
npm run dev      # http://localhost:4321/homepage/
npm run build    # writes dist/
npm run preview  # serve dist/
```

## Deploy on GitHub Pages

The workflow at `.github/workflows/deploy.yml` builds and deploys on every
push to `main`. To turn it on:

1. Merge this branch to `main`.
2. Repo Settings → Pages → Source: **GitHub Actions** (not "Deploy from a branch").
3. Push to `main`. The workflow runs and publishes
   `https://jcharmer.github.io/homepage/`.

Custom domain (later): add a `CNAME` file at repo root with the domain, set
the DNS records GitHub shows you, and update `site` in `astro.config.mjs`.

## Known issues / next steps

- The gallery's inline CSS references `--cd-border` and `--cd-primary`,
  which aren't defined in `colors_and_type.css`. Borders render transparent
  and the primary color falls back. Map them to `--cd-rule` and `--cd-green`
  (or add them to the token file).
- Pick a landing variant if you don't want the gallery to be `/`.
- When you're ready to share headers/footers/nav across variants: convert
  `public/v[N]/index.html` → `src/pages/v[N].astro` using `Layout.astro`.

## Branch

Working branch: `claude/review-repo-info-EH95P`. The repo has no `main`
branch yet — first push of this branch (or a merge) creates it.
