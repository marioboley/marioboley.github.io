# Software Collection Design

**Date:** 2026-04-03

## Goal

Add a Software collection to the site, reachable via top-level navigation, with fastridge as the first item. Publications can link to a software item via a `softwareurl` field in their summary links row.

## Scope

- New `_software/` collection with its own layout and archive page
- New front matter field `softwareurl` on publications
- First software item: fastridge (Python package)
- Does NOT include: structured cross-linking from publications to software (deferred), refactoring of `single.html` publication links into a shared include (deferred), R implementation of fastridge (not currently locatable)

## Targeted Future Design

This project establishes a pattern intended to generalise:
- Each collection gets its own layout (`_layouts/software.html`, eventually `_layouts/publication.html`)
- Each collection gets its own links include (`_includes/software-links.html`, eventually `_includes/publication-links.html`)
- Each collection gets its own archive-single include (`_includes/archive-single-software.html`, already exists for publications)
- The links include is the shared unit: used by both the full-page layout and the archive-single include

When the publication refactor happens, `_includes/publication-links.html` will be extracted from `single.html` and reused in `archive-single-publication.html` following the same pattern.

## Data Model

### Software front matter (`_software/YYYY-slug.md`)

- Required: `title`, `collection`, `permalink`
- Optional: `sourceurl` (source code repository, e.g. GitHub), `packageurl` (package index, e.g. PyPI), `publications` (list of publication slugs for cross-linking)

### Publication front matter addition

- `softwareurl`: single URL pointing to the on-site software page (preferred) or directly to an external repository. Decided case by case. Renders a "Software" link in the Paper / Slides / Video / Software / BibTeX row on the publication page.

## Architecture

### New files

**`_layouts/software.html`** — layout for individual software pages. Extends `default.html` (parallel to `single.html`, not inheriting from it). Renders page chrome (title, sidebar, content), calls `_includes/software-links.html` for the Source / Package link row, then calls `_includes/related_publications.html` for the publications slug list. Some structural duplication with `single.html` is accepted now; future refactor extracts common chrome into a shared include.

**`_includes/software-links.html`** — renders the Source and Package links conditionally from `page.sourceurl` and `page.packageurl`. Shared between `software.html` and `archive-single-software.html` to ensure the links row is consistent in both contexts.

**`_includes/archive-single-software.html`** — renders a compact summary row for use on the software archive page: title (linked to the software page) followed by `software-links.html`.

**`_pages/software.html`** — archive page listing all software items using `archive-single-software.html`. Layout: archive, permalink: `/software/`.

**`_software/fastridge.md`** — first software item:
```yaml
title: "fastridge"
collection: software
permalink: /software/fastridge/
sourceurl: 'https://github.com/marioboley/fastridge'
packageurl: 'https://pypi.org/project/fastridge/'
publications:
  - 2023-bayesbeatscv
```
Body: brief description of what fastridge is.

### Modified files

**`_config.yml`** — add `software` to collections (output: true, permalink: `/:collection/:path/`) and a defaults entry (layout: software, author_profile: true).

**`_data/navigation.yml`** — add "Software" entry before "Projects".

**`_layouts/single.html`** — add `softwareurl` → "Software" link in the existing Paper / Slides / Video / BibTeX row, after Video and before BibTeX. Opens in new tab with `rel="noopener"` (consistent with Video link).

**`_publications/2023-bayesbeatscv.md`** — add `softwareurl: '/software/fastridge/'`.

## Potential Future Work

- Extract `_includes/publication-links.html` from `single.html` and reuse in `archive-single-publication.html`
- Create `_layouts/publication.html` as the publication-specific layout
- Add structured `software` slug list to publications for a "Related Software" section (symmetric cross-linking)
- Add version release blog posts as site updates via `_posts/`
- Add R implementation of fastridge once locatable
