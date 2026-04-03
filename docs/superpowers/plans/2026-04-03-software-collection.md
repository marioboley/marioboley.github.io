# Software Collection Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a Software collection with its own layout, archive page, and navigation entry; establish a shared `software-links.html` include used by both the layout and archive summary; add `softwareurl` to publication pages; populate with fastridge as the first item.

**Architecture:** `_includes/software-links.html` is the shared unit rendering Source / Package links, called by both `_layouts/software.html` (full page) and `_includes/archive-single-software.html` (list summary). This mirrors the targeted future pattern for all collections. `_layouts/single.html` gains a `softwareurl` field for the publication links row — this is independent of the collection and done first. The talks collection already uses a custom layout (`layout: talk`), confirming the per-collection layout pattern is established in the codebase.

**Tech Stack:** Jekyll, Liquid templating

---

### Task 1: Add `softwareurl` to `_layouts/single.html` and `_publications/2023-bayesbeatscv.md`

**Files:**
- Modify: `_layouts/single.html:52,69-75`
- Modify: `_publications/2023-bayesbeatscv.md`

Adds a "Software" link after "Video" in the publication links row. Also fixes the outer condition on line 52 which currently omits `videourl` and needs `softwareurl` added too.

- [ ] **Step 1: Fix the outer condition and add Software link in `_layouts/single.html`**

Replace line 52:
```liquid
        {% if page.citation or page.paperurl or page.fulltexturl or page.slidesurl or page.bibtex %}
```
with:
```liquid
        {% if page.citation or page.paperurl or page.fulltexturl or page.slidesurl or page.videourl or page.softwareurl or page.bibtex %}
```

After the `videourl` block (lines 69–72), insert before the `bibtex` block:

```liquid
            {% if page.softwareurl %}
              {% if page.paperurl or page.fulltexturl or page.slidesurl or page.videourl %} | {% endif %}
              <a href="{{ page.softwareurl }}" target="_blank" rel="noopener">Software</a>
            {% endif %}
```

Update the BibTeX separator (line 75) to include `softwareurl`:
```liquid
              {% if page.paperurl or page.fulltexturl or page.slidesurl or page.videourl or page.softwareurl %} | {% endif %}
```

- [ ] **Step 2: Add `softwareurl` to `_publications/2023-bayesbeatscv.md`**

In the front matter, add after `videourl`:
```yaml
softwareurl: '/software/fastridge/'
```

- [ ] **Step 3: Verify in browser**

Check `http://localhost:4000/publication/2023-bayesbeatscv` — the links row should show `Paper | Video | Software | BibTeX`. The Software link opens `/software/fastridge/` (returns 404 for now — the collection doesn't exist yet, that's expected).

- [ ] **Step 4: Commit**

```bash
git add _layouts/single.html _publications/2023-bayesbeatscv.md
git commit -m "feat: add softwareurl field to publication layout; link bayesbeatscv to fastridge"
```

---

### Task 2: Create `_includes/software-links.html`

**Files:**
- Create: `_includes/software-links.html`

This include renders Source and Package links from an `item` parameter. Using an explicit parameter avoids ambiguity between `page` (the current page in layout context) and loop variables (in archive context). Called by both `archive-single-software.html` (passing the loop post) and `software.html` (passing `page`).

- [ ] **Step 1: Create the file**

```html
{%- assign item = include.item -%}
{% if item.sourceurl or item.packageurl %}
  <p style="font-size: smaller">
    {% if item.sourceurl %}
      <a href="{{ item.sourceurl }}" target="_blank" rel="noopener">Source</a>
    {% endif %}
    {% if item.packageurl %}
      {% if item.sourceurl %} | {% endif %}
      <a href="{{ item.packageurl }}" target="_blank" rel="noopener">Package</a>
    {% endif %}
  </p>
{% endif %}
```

- [ ] **Step 2: Commit**

```bash
git add _includes/software-links.html
git commit -m "feat: add software-links include for Source/Package link row"
```

---

### Task 3: Create `_includes/archive-single-software.html`

**Files:**
- Create: `_includes/archive-single-software.html`

Renders a compact summary row for the software archive page: title linked to the software page, followed by the software links block. Follows the same `include.post | default: post` pattern as `_includes/archive-single-publication.html`.

- [ ] **Step 1: Create the file**

```html
{%- assign sw = include.post | default: post -%}

<p itemprop="description">
  <a href="{{ sw.url | relative_url }}">{{ sw.title }}</a>
</p>
{% include software-links.html item=sw %}
```

- [ ] **Step 2: Commit**

```bash
git add _includes/archive-single-software.html
git commit -m "feat: add archive-single-software include for software list view"
```

---

### Task 4: Create `_layouts/software.html`

**Files:**
- Create: `_layouts/software.html`

Full-page layout for individual software items. Extends `default` directly (parallel to `single.html` — same pattern as the existing `talk` layout). Renders title, page content, software links, then related publications. The `related_publications.html` include uses `page.publications` (the slug list on the software item) to resolve and render related papers.

- [ ] **Step 1: Create the file**

```html
---
layout: default
---

{% include base_path %}

{% if page.header.overlay_color or page.header.overlay_image or page.header.image %}
  {% include page__hero.html %}
{% endif %}

{% if page.url != "/" and site.breadcrumbs %}
  {% unless paginator %}
    {% include breadcrumbs.html %}
  {% endunless %}
{% endif %}

<div id="main" role="main">
  {% include sidebar.html %}

  <article class="page" itemscope itemtype="http://schema.org/CreativeWork">
    {% if page.title %}<meta itemprop="headline" content="{{ page.title | markdownify | strip_html | strip_newlines | escape_once }}">{% endif %}
    {% if page.excerpt %}<meta itemprop="description" content="{{ page.excerpt | markdownify | strip_html | strip_newlines | escape_once }}">{% endif %}

    <div class="page__inner-wrap">
      {% unless page.header.overlay_color or page.header.overlay_image %}
        <header>
          {% if page.title %}<h1 class="page__title" itemprop="headline">{{ page.title | markdownify | remove: "<p>" | remove: "</p>" }}</h1>{% endif %}
        </header>
      {% endunless %}

      <section class="page__content" itemprop="text">
        {{ content }}

        {% include software-links.html item=page %}

        {% include related_publications.html %}
      </section>

      <footer class="page__meta">
        {% if site.data.ui-text[site.locale].meta_label %}
          <h4 class="page__meta-title">{{ site.data.ui-text[site.locale].meta_label }}</h4>
        {% endif %}
        {% include page__taxonomy.html %}
      </footer>

      {% include post_pagination.html %}
    </div>
  </article>
</div>
```

- [ ] **Step 2: Commit**

```bash
git add _layouts/software.html
git commit -m "feat: add software layout extending default"
```

---

### Task 5: Register collection, add navigation, create archive page and first item

**Files:**
- Modify: `_config.yml`
- Modify: `_data/navigation.yml`
- Create: `_pages/software.html`
- Create: `_software/fastridge.md`

- [ ] **Step 1: Add collection to `_config.yml`**

In the `collections:` block (around line 228), add after `projects:`:

```yaml
  software:
    output: true
    permalink: /:collection/:path/
```

- [ ] **Step 2: Add defaults entry to `_config.yml`**

In the `defaults:` block, add after the `_talks` entry (around line 293):

```yaml
  # _software
  - scope:
      path: ""
      type: software
    values:
      layout: software
      author_profile: true
```

- [ ] **Step 3: Add navigation entry to `_data/navigation.yml`**

Insert before the "Projects" entry:

```yaml
  - title: "Software"
    url: /software/
```

- [ ] **Step 4: Create `_pages/software.html`**

```html
---
layout: archive
title: "Software"
permalink: /software/
author_profile: true
---

{% include base_path %}

{% for post in site.software reversed %}
  {% include archive-single-software.html %}
{% endfor %}
```

- [ ] **Step 5: Create `_software/fastridge.md`**

```markdown
---
title: "fastridge"
collection: software
permalink: /software/fastridge/
sourceurl: 'https://github.com/marioboley/fastridge'
packageurl: 'https://pypi.org/project/fastridge/'
publications:
  - 2023-bayesbeatscv
---

Python implementation of fast ridge regression via expectation maximization. Provides efficient joint estimation of the regularization parameter and regression coefficients without cross-validation, with O(min(n,p)) cost per EM iteration after preprocessing.
```

- [ ] **Step 6: Verify in browser**

Check `http://localhost:4000/software/` — should show fastridge with title, Source, and Package links.

Check `http://localhost:4000/software/fastridge/` — should show description, Source / Package links, and Related Publications listing the bayesbeatscv paper.

Check navigation bar — "Software" should appear before "Projects".

Check `http://localhost:4000/publication/2023-bayesbeatscv` — the Software link should now resolve correctly (no longer 404).

- [ ] **Step 7: Commit**

```bash
git add _config.yml _data/navigation.yml _pages/software.html _software/fastridge.md
git commit -m "feat: add software collection, archive page, nav entry, and fastridge item"
```

---

### Task 6: Update `CLAUDE.md`

**Files:**
- Modify: `CLAUDE.md`

- [ ] **Step 1: Update the publications Optional fields line**

Replace the existing Optional fields line with:

```
- Optional: `excerpt`, `paperurl`, `fulltexturl` (overrides `paperurl` for the "Paper" link when present), `slidesurl`, `videourl` (URL to video recording, local or external; renders a "Video" link that opens in a new tab), `softwareurl` (URL to on-site software page or external repo; renders a "Software" link that opens in a new tab), `bibtex`
```

- [ ] **Step 2: Add Software collection section after the Projects section**

```markdown
**Software** (`_software/slug.md`) — front matter fields:
- Required: `title`, `collection`, `permalink`
- Optional: `sourceurl` (source repository URL; renders a "Source" link opening in a new tab), `packageurl` (package index URL e.g. PyPI; renders a "Package" link opening in a new tab), `publications` (list of publication slugs for cross-linking to related papers)

The `_includes/software-links.html` partial renders the Source / Package link row and is shared between `_layouts/software.html` and `_includes/archive-single-software.html`.
```

- [ ] **Step 3: Commit and push**

```bash
git add CLAUDE.md
git commit -m "docs: document software collection and softwareurl publication field"
git push
```
