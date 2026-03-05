# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repo Structure

There are two portfolio repos in play:

- **`/Users/tobin/code/joshuatobin/`** — `joshuatobin/joshuatobin` on GitHub. Original Render-targeted static Astro site. Largely superseded.
- **`/Users/tobin/code/joshuatobin/cf-portfolio/`** — `joshuatobin/portfolio` on GitHub. **Active site.** Astro + Cloudflare Workers adapter, deployed to `joshuatobin.ai` via Cloudflare. All content changes go here.

## Active Site — cf-portfolio

### Commands

```bash
cd cf-portfolio

npm run dev          # local dev server
npm run build        # production build (required before push to verify)
npm run preview      # build + wrangler dev (Cloudflare Workers emulation)
npm run deploy       # deploy via wrangler (Cloudflare handles this on push)
```

Always run `npm run build` before committing to catch errors.

### Architecture

The entire portfolio is a **single file**: `src/pages/index.astro`.

All content — experience, stats, projects, skills — is defined as TypeScript data arrays at the top of the file in the frontmatter (`---` block). To update content, edit those arrays; no other files need touching.

Inline `<style>` in the same file handles all styling using CSS custom properties. Tailwind is available but the existing styles use raw CSS variables for the design system (`--brand`, `--bg`, `--surface`, `--border`, etc.).

The Cloudflare adapter (`@astrojs/cloudflare`) enables Workers deployment. `wrangler.json` at the root of `cf-portfolio` contains the Worker config. No secrets are committed — any needed secrets use `wrangler secret put`.

### Deployment

Cloudflare auto-deploys on push to `main` of `joshuatobin/portfolio`. No manual deploy step needed. Domain: `joshuatobin.ai`.

### Content Conventions

- All project GitHub links are omitted (repos are private)
- Project stats badges use the `stats: []` array — use short strings like `"Alpha"`, `"In development"`, `"Coming soon"`
- The `url` field on projects is the live link shown as "↗ Live" — set to `null` if not yet live
- Availability copy is in the `#contact` section — currently full-time only, no contracts
