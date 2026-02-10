# Project Notes

## Local Development

- Run `bundle exec jekyll serve` to start the local server at `http://127.0.0.1:4000`
- Changes to most files (posts, pages, includes, layouts) are picked up automatically via auto-regeneration
- Changes to `_config.yml` require a full server restart (kill and re-run `bundle exec jekyll serve`)
- If port 4000 is already in use: `lsof -ti:4000 | xargs kill -9` then restart
- Sass deprecation warnings from the Minima theme are harmless and can be ignored

## Theme Setup

- Uses Minima v3 via `remote_theme: jekyll/minima` for GitHub Pages, and `theme: minima` with `gem "minima", github: "jekyll/minima"` in the Gemfile for local dev
- `remote_theme` requires `jekyll-remote-theme` plugin on GitHub Pages (auto-enabled), but locally it can fail due to SSL issues so the gem-from-git approach is used instead
- `jekyll-remote-theme` should NOT be in the plugins list in `_config.yml` since it's not in the Gemfile for local dev
- Dark mode uses `skin: auto` which follows OS preference via `prefers-color-scheme`
- Minima's custom head extension point is `_includes/custom-head.html` (not `head-custom.html` like Cayman)

## Creating new blog posts

When creating posts only prepopulate with headings and writing prompts. Do not write any significant content.

## Overridden Theme Files

- `_includes/header.html` - adds social link icons (GitHub, LinkedIn) to the header
- `_includes/custom-head.html` - Mermaid.js loading and header social link styling
- `_layouts/home.html` - adds "Read more" links after post excerpts


## Mermaid Diagrams

- Use fenced code blocks with `mermaid` language tag
- Mermaid.js is loaded from CDN in `_includes/custom-head.html`
- Targets `.language-mermaid` class (how Jekyll renders mermaid code blocks)
