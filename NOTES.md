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
astro.config.mjs             site=https://jcharmer.github.io (User Pages, root)
.github/workflows/deploy.yml GitHub Pages deploy (Astro action)
```

The variant pages (v1–v10) are still raw HTML with inline CSS — they were
self-contained, so they live in `public/` and Astro serves them as-is at
`/v1/`, `/v2/`, etc. Convert them to `.astro` later when you want to share
chrome between them.

## Local development

```sh
npm install
npm run dev      # http://localhost:4321/
npm run build    # writes dist/
npm run preview  # serve dist/
```

## Deploy on GitHub Pages

This is a User Pages repo (`jcharmer/jcharmer.github.io`), so the site
publishes at the root: `https://jcharmer.github.io/`.

The workflow at `.github/workflows/deploy.yml` builds and deploys on every
push to `main`. Make sure Repo Settings → Pages → Source is set to
**GitHub Actions** (not "Deploy from a branch").

## Custom domain setup (when ready)

1. Buy a domain at any registrar (Cloudflare, Namecheap, Porkbun, etc).
2. Add DNS records at the registrar:
   - **Apex (`example.com`)**: four A records to GitHub's IPs:
     ```
     185.199.108.153
     185.199.109.153
     185.199.110.153
     185.199.111.153
     ```
     Plus a `www` CNAME → `jcharmer.github.io` if you want both to work.
   - **Subdomain only (`www.example.com`, `site.example.com`, etc.)**: one
     CNAME record → `jcharmer.github.io`.
3. Add a `CNAME` file at `public/CNAME` with the domain on one line
   (no protocol, no trailing slash). It must live in `public/` so Astro
   copies it to `dist/` on every build — otherwise the deploy wipes it out
   and GitHub drops the custom-domain config.
4. Update `site` in `astro.config.mjs` to the new domain (e.g.
   `https://example.com`). Affects sitemap and absolute URLs.
5. Repo Settings → Pages → Custom domain: enter the domain. After DNS
   resolves, turn on **Enforce HTTPS**.

DNS propagation usually takes 5–60 minutes; Cloudflare is near-instant.

## Known issues / next steps

- The gallery's inline CSS references `--cd-border` and `--cd-primary`,
  which aren't defined in `colors_and_type.css`. Borders render transparent
  and the primary color falls back. Map them to `--cd-rule` and `--cd-green`
  (or add them to the token file).
- Pick a landing variant if you don't want the gallery to be `/`.
- When you're ready to share headers/footers/nav across variants: convert
  `public/v[N]/index.html` → `src/pages/v[N].astro` using `Layout.astro`.

## Branch

Default branch: `main`. Feature work happens on `claude/*` branches.
