# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Personal portfolio site for Peter Manahan, built with [Zola](https://www.getzola.org/) — a Rust static site generator. No JavaScript build step, no Node.js. Zola handles Sass compilation and templating natively.

## Commands

```bash
zola serve          # dev server at http://127.0.0.1:1111 (live reload)
zola build          # build to public/
```

Install Zola if needed: `brew install zola`

## Architecture

**Templating** uses Zola's [Tera](https://keats.github.io/tera/) engine (Jinja2-like syntax). All templates extend `templates/base.html`.

**Content** is Markdown with TOML front matter (`+++` delimiters). Zola's content model:
- A file like `content/about.md` maps to `/about/`
- A directory with `_index.md` is a **section** (e.g. `content/projects/`)
- Pages inside a section are **pages**; `projects/_index.md` uses `sort_by = "weight"` so project ordering is controlled by the `weight` field in each project's front matter

**Project pages** use `[extra]` front matter fields (`summary`, `tags`, `repo`) that are accessed in templates via `page.extra.summary`, etc. The projects listing template (`templates/projects.html`) iterates `section.pages`.

**Styles** live entirely in `sass/style.scss` — a single file, dark theme, no partials. Zola compiles it automatically (no separate Sass CLI needed). The compiled output is referenced as `style.css` via `get_url(path='style.css')` in the base template.

**Static assets** in `static/` are served as-is (e.g. `static/Peter_Manahan_Resume.pdf` → `/Peter_Manahan_Resume.pdf`).

**Template–content mapping**: content files declare which template to use via the `template` field in front matter (e.g. `template = "index.html"`). Sections can also declare a template in `_index.md`.

### Templates (`templates/`)

| Template | URL | Notes |
|---|---|---|
| `base.html` | — | Root layout; header/nav/footer; all others extend it |
| `index.html` | `/` | Hero with name, tagline, and 3 CTA buttons |
| `about.html` | `/about/` | Title + rendered Markdown body |
| `contact.html` | `/contact/` | Rendered Markdown body inside `.contact-page` div |
| `resume.html` | `/resume/` | **Hardcoded HTML** — not rendered from Markdown; edit template directly |
| `projects.html` | `/projects/` | Card listing, iterates `section.pages` sorted by `weight` |
| `page.html` | `/projects/<slug>/` | Individual project; renders title, tags, repo link, Markdown body |

`base.html` includes: Google Fonts (Inter 400–700, JetBrains Mono 400–500), top navigation (Projects, About, Resume, Contact), and a footer with a dynamic copyright year via `{{ now() | date(format="%Y") }}`.

### Content & Front Matter

**Root pages** (e.g. `content/about.md`):
```toml
+++
title = "About"
path = "about"
template = "about.html"
+++

Markdown body here...
```

**Project pages** (e.g. `content/projects/factory-calc.md`):
```toml
+++
title = "Factory Calculator"
weight = 1
[extra]
summary = "Short description shown on the projects listing."
tags = ["Rust", "CLI", "Game Tools"]
repo = "https://github.com/pmanahan/factory_calc"
+++

Markdown body here (shown on the project detail page)...
```

- `weight` controls display order (ascending); lower number = listed first
- `summary` and `tags` are optional but expected by `projects.html` (guarded with `if` checks)
- `repo` is optional; `page.html` renders a GitHub link button only if present
- `resume.html` ignores its Markdown body entirely — all content is in the template

## Design

**Color palette** (defined as Sass variables in `sass/style.scss`):

| Variable | Value | Usage |
|---|---|---|
| `$bg` | `#0f1419` | Page background |
| `$surface` | `#1a1f2e` | Cards, code blocks |
| `$border` | `#2a3040` | Borders |
| `$text` | `#c9d1d9` | Body text |
| `$text-muted` | `#8b949e` | Secondary/muted text |
| `$heading` | `#e6edf3` | Headings |
| `$accent` | `#58a6ff` | Links, buttons, tags |
| `$accent-hover` | `#79b8ff` | Hover state for accent elements |
| `$tag-bg` | `#1c2433` | Tag badge background |

**Fonts**: Inter (sans-serif body), JetBrains Mono (monospace/code). Both loaded from Google Fonts in `base.html`.

**Responsive**: A single breakpoint at `@media (max-width: 600px)` reduces base font size to 16px, stacks the header vertically, and shrinks the hero `h1` to 2rem.

**Max content width**: 720px (`.container` class).

## Key config (`config.toml`)

- `base_url = "https://pmanahan.dev"`
- `compile_sass = true` — Sass is compiled by Zola, not a separate tool
- `minify_html = true`
- `build_search_index = false`, `generate_feeds = false`
- Syntax highlighting is under `[markdown.highlighting]` with `highlight_code = true` and `theme = "ayu-dark"` (Zola 0.22 moved highlighting out of `[markdown]` into its own subsection; available themes include `ayu-dark`, `dracula`, `github-dark`, `catppuccin-mocha`, `solarized-dark`, `nord`, `one-light`, etc.)
- `[extra]` block holds `author = "Peter Manahan"` and `email = "prmanahan@pm.me"`, accessible in templates as `config.extra.author` and `config.extra.email`
