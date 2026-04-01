# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About this project

Static portfolio site for Louis Kempson-Seaton (AI & Business Automation specialist). No build tools, no framework, no package manager — pure HTML/CSS/JS.

## Development

Open any `.html` file directly in a browser, or serve locally:

```bash
python3 -m http.server 8080
# or
npx serve .
```

No build step. No compilation. No install step.

## Architecture

**4 pages** (index, about, projects, cv) — each is a standalone HTML file with the full `<nav>` duplicated inline. There is no templating system; navbar changes must be applied to all 4 files.

**`css/style.css`** — single stylesheet for the entire site. Structured as: CSS variables → resets → layout → nav → components (hero, cards, timeline, skills grid, workflow canvas) → animations → responsive breakpoints (768px).

**`js/main.js`** — 48 lines. Handles mobile nav toggle, scroll-triggered `.fade-in` animations via Intersection Observer, and active nav link highlighting.

**CSS variables** control the design system. Key ones:
- `--bg-primary: #09090b`, `--card-bg: #15151a` — dark theme backgrounds
- `--accent: #3b82f6` — blue accent
- Typography: JetBrains Mono (headings) + Geist Sans (body), loaded from Google Fonts

**The SVG workflow diagram** on the homepage is inline in `index.html` and styled by `.workflow-canvas` rules in style.css. It mimics n8n-style node graphs.

**Scroll animations** — add class `fade-in` to any element to have it animate in on scroll. The Intersection Observer in main.js handles this automatically.

## Hosting & Deployment

**Stack:** nginx → Cloudflare Tunnel → internet. No build or deploy step.

- Nginx serves the repo directory directly: `root /home/zampano/projects/kempsonseaton.com` on `127.0.0.1:8200`
- Cloudflare Tunnel (`cloudflared`, systemd service) exposes port 8200 to `kempsonseaton.com`
- **Merging to `main` updates the live site instantly** — nginx reads files on every request

**To preview changes locally before merging:**
```bash
python3 -m http.server 8080
# open http://192.168.1.118:8080 from any device on the local network
```

**Git remote:** `https://github.com/zampano95/kempsonseaton.com.git`
