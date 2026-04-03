# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Local Development

Requires Ruby 3.x via rbenv (`.ruby-version` is set in the repo root):

```bash
bundle install
bundle exec jekyll serve        # serve at http://localhost:4000
bundle exec jekyll serve -l     # with live reload
bundle exec jekyll build        # build to _site/
```

Note: Jekyll's WEBrick server does not support HTTP range requests, so `<video>` elements won't play locally. To test video, either push to GitHub Pages or build and serve with:

```bash
bundle exec jekyll build && cd _site && python3 -m http.server 4000
```

## Architecture

This is an **Academic Pages** Jekyll site (fork of Minimal Mistakes). Content lives in five collections (`_publications/`, `_talks/`, `_projects/`, `_teaching/`, `_portfolio/`), rendered via `_layouts/single.html` and aggregated by pages in `_pages/`.

### Content Collections

**Publications** (`_publications/YYYY-slug.md`) — front matter fields:
- Required: `title`, `collection`, `category` (`manuscripts` | `conferences` | `books`), `permalink`, `date`, `venue`, `citation`
- Optional: `excerpt`, `paperurl`, `fulltexturl` (overrides `paperurl` for the "Paper" link when present), `slidesurl`, `videourl` (URL to video recording, local or external; renders a "Video" link that opens in a new tab), `bibtex`

**Talks** (`_talks/YYYY-MM-DD-slug.md`) — fields: `title`, `type`, `venue`, `date`, `location`, `slidesurl`

**Projects** (`_projects/YYYY-slug.md`) — fields: `title`, `date`, `project_type` (`collaborative` | `student`), `publications` (list of publication slugs for cross-linking)

### Key Layout: `_layouts/single.html`

Handles all publication/talk/project pages. Renders citation, Paper/Slides/BibTeX links, and an expandable BibTeX block with copy-to-clipboard. The `fulltexturl` field was added to support locally-hosted PDFs as an alternative to paywalled `paperurl` links.

### Publications Page Logic

`_pages/publications.html` groups publications by the `category` front matter field, using the `publication_category` mapping defined in `_config.yml`:
```yaml
publication_category:
  manuscripts: Journal Articles
  conferences: Conference Papers
  books: Books
```

### Cross-Collection Linking

Projects can list related publications by slug in their `publications` front matter array. `_includes/related_publications.html` resolves these slugs and renders links inline.

### Static Files

PDFs, slides, and videos are stored in `files/` with the naming convention `YYYY-MM-DD-type-author-slug.ext`. Reference them with absolute paths like `/files/filename.ext`.

### Jekyll Exclusions

The `jekyll-optional-front-matter` plugin causes Jekyll to process **all** markdown files in the repo, even without front matter. Liquid tags inside code blocks are evaluated before markdown rendering, so fenced code blocks do **not** protect Liquid syntax — this can crash the GitHub Pages build silently.

Any new directory containing markdown files that is **not** a Jekyll collection (i.e. not a `_`-prefixed directory like `_publications/`) must be added to the `exclude` list in `_config.yml`. New collections added under `_` prefixed directories are handled correctly by Jekyll and do not need excluding. Currently excluded non-collection directories: `docs/`, `CLAUDE.md`.

### Batch Content Generation

`markdown_generator/` contains Python/Jupyter tools to bulk-generate markdown from TSV or BibTeX input (`pubsFromBib.py`, `OrcidToBib.ipynb`, etc.). Prefer these for adding multiple entries at once.
