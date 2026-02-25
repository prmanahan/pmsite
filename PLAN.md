# pmsite — Project Plan

Personal portfolio site for Peter Manahan.
Built with **Zola** (Rust static site generator).

---

## Current State

The Zola project has been scaffolded with:

- `config.toml` — site config (base URL, title, Sass compilation, syntax highlighting)
- `content/` — Markdown content for all pages (About, Resume, Contact, Projects)
- `templates/` — HTML templates (base layout, index, about, projects, resume, contact, page)
- `sass/style.scss` — full stylesheet (dark theme, responsive, project cards, resume layout)
- `static/Peter_Manahan_Resume.pdf` — downloadable resume PDF
- 3 project entries: Factory Calculator, Vibe Calculator, Learning Bevy

The site should build and run with `zola serve` as-is.

---

## What Still Needs Doing

### 1. Install Zola locally and verify the build
- `brew install zola` (or `cargo install zola`)
- `cd pmsite && zola serve` — confirm it renders at localhost:1111
- Fix any template or Sass issues that surface

### 2. Review and personalize content
- **About page** — edit `content/about.md` to match your voice; current text is a draft based on your resume
- **Project descriptions** — update `content/projects/*.md` with more detail, screenshots, or links
- **Contact page** — add or remove contact methods (GitHub username, etc.)
- **Resume page** — check the HTML resume template (`templates/resume.html`) matches your PDF; update as career evolves

### 3. Polish the design
- Tune colors, spacing, and typography in `sass/style.scss`
- Add a favicon (`static/favicon.ico`)
- Consider adding an avatar/photo to the hero or about page
- Test mobile responsiveness

### 4. Add optional features
- **Blog section** — `content/blog/_index.md` + posts if you want to write
- **Syntax-highlighted code samples** in project pages
- **RSS feed** — set `generate_feeds = true` in config.toml
- **Search** — Zola has built-in search index support
- **Analytics** — add a privacy-respecting option (Plausible, Umami) if desired

### 5. Deploy
- Pick a host: **GitHub Pages**, **Cloudflare Pages**, **Netlify**, or **Vercel** (all support Zola)
- Set up a GitHub repo and push
- Configure custom domain (e.g., `pmanahan.dev`) with DNS
- Set up CI: most hosts auto-build on push; alternatively add a GitHub Action for `zola build`

### 6. Ongoing maintenance
- Add new projects as you build them
- Keep resume content in sync with the PDF
- Write blog posts if the blog section is added

---

## File Structure

```
pmsite/
├── config.toml
├── PLAN.md                  ← this file
├── content/
│   ├── _index.md            ← home page
│   ├── about.md
│   ├── contact.md
│   ├── resume.md
│   └── projects/
│       ├── _index.md        ← projects listing
│       ├── factory-calc.md
│       ├── bevy-learning.md
│       └── vibe-calc.md
├── templates/
│   ├── base.html            ← shared layout (header, nav, footer)
│   ├── index.html           ← home page hero
│   ├── about.html
│   ├── contact.html
│   ├── resume.html          ← full resume in HTML
│   ├── projects.html        ← project listing with cards
│   └── page.html            ← individual project detail
├── sass/
│   └── style.scss           ← all styles (dark theme, responsive)
└── static/
    ├── Peter_Manahan_Resume.pdf
    └── images/              ← add screenshots, avatar, etc.
```

---

## Quick Start

```bash
brew install zola        # install Zola
cd pmsite
zola serve               # dev server at http://127.0.0.1:1111
zola build               # output to public/
```
