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

## Key config (`config.toml`)

- `base_url = "https://pmanahan.dev"`
- `compile_sass = true` — Sass is compiled by Zola, not a separate tool
- `minify_html = true`
- Syntax highlighting is under `[markdown.highlighting]` with `highlight_code = true` and `theme = "ayu-dark"` (Zola 0.22 moved highlighting out of `[markdown]` into its own subsection; available themes include `ayu-dark`, `dracula`, `github-dark`, `catppuccin-mocha`, `solarized-dark`, `nord`, `one-light`, etc.)
- `[extra]` block holds `author` and `email`, accessible in templates as `config.extra.author`
