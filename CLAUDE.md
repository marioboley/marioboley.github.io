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

This is an **Academic Pages** Jekyll site (fork of Minimal Mistakes). Content lives in six collections (`_publications/`, `_software/`, `_talks/`, `_projects/`, `_teaching/`, `_portfolio/`), aggregated by pages in `_pages/`. Collections use either a dedicated layout (`_layouts/software.html`, `_layouts/talk.html`) or the general-purpose `_layouts/single.html`, which currently serves publications, projects, and teaching pages. The latter are flagged for future refactoring into dedicated layouts following the same pattern.

### Content Collections

**Publications** (`_publications/YYYY-slug.md`) — front matter fields:
- Required: `title`, `collection`, `category` (`manuscripts` | `conferences` | `books`), `permalink`, `date`, `venue`, `citation`
- Optional: `excerpt`, `paperurl`, `fulltexturl` (overrides `paperurl` for the "Paper" link when present), `slidesurl`, `videourl` (URL to video recording, local or external; renders a "Video" link that opens in a new tab), `softwareurl` (URL to on-site software page or external repo; renders a "Software" link to the same tab), `bibtex`

**Software** (`_software/slug.md`) — front matter fields:
- Required: `title`, `collection`, `permalink`
- Optional: `excerpt`, `sourceurl` (source repository URL; renders a "Source" link opening in a new tab), `packageurl` (package index URL e.g. PyPI; renders a "Package" link opening in a new tab), `publications` (list of publication slugs for cross-linking)

The `_includes/software-links.html` partial renders the Source / Package link row and is shared between `_layouts/software.html` and `_includes/archive-single-software.html`.

**Talks** (`_talks/YYYY-MM-DD-slug.md`) — fields: `title`, `type`, `venue`, `date`, `location`, `slidesurl`

**Projects** (`_projects/YYYY-slug.md`) — fields: `title`, `date`, `project_type` (`collaborative` | `student`), `publications` (list of publication slugs for cross-linking)

### Layouts

**`_layouts/single.html`** — handles publications, projects, and teaching pages. Renders citation and a Paper / Slides / Video / Software / BibTeX link row (each conditional on its front matter field), plus an expandable BibTeX block with copy-to-clipboard. The `fulltexturl` field overrides `paperurl` for the Paper link. The link row is inlined in the layout rather than extracted into a shared include — flagged for refactoring to follow the pattern established by `_includes/software-links.html`. Flagged for eventual splitting into dedicated per-collection layouts.

**`_layouts/software.html`** — handles software pages. Renders page content, calls `_includes/software-links.html` for the Source / Package link row, then `_includes/related_publications.html` for cross-linked papers.

**`_layouts/talk.html`** — handles talk pages.

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
