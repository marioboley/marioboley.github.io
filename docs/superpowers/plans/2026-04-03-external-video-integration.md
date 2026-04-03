# External Video Integration Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a `videourl` front matter field that renders a "Video" link in the Paper / Slides / BibTeX row on individual publication pages, and apply it to the 2023 NeurIPS bayesbeatscv paper with a SlidesLive embed.

**Architecture:** Two files are touched. `_layouts/single.html` gains a conditional `| Video` link driven by `page.videourl`, inserted after Slides and before BibTeX. `_publications/2023-bayesbeatscv.md` gains `videourl` in its front matter and a SlidesLive `<iframe>` in its body. No new files are created.

**Tech Stack:** Jekyll (Liquid templating), HTML

---

### Task 1: Add Video link to `_layouts/single.html`

**Files:**
- Modify: `_layouts/single.html:64-78`

The existing Slides block ends at around line 67. The BibTeX block begins at around line 69. Insert the Video block between them. The separator logic must follow the same pattern as Slides and BibTeX: include ` | ` only when at least one earlier link exists.

- [ ] **Step 1: Open `_layouts/single.html` and locate the links block**

The relevant section (lines 52–78) currently looks like:

```liquid
{% if page.slidesurl %}
  {% if page.paperurl or page.fulltexturl %} | {% endif %}
  <a href="{{ page.slidesurl }}">Slides</a>
{% endif %}

{% if page.bibtex %}
  {% if page.paperurl or page.fulltexturl or page.slidesurl %} | {% endif %}
  <a href="#" onclick="...">BibTeX</a>
{% endif %}
```

- [ ] **Step 2: Insert the Video block between Slides and BibTeX**

Replace the Slides block and the BibTeX separator line with:

```liquid
            {% if page.slidesurl %}
              {% if page.paperurl or page.fulltexturl %} | {% endif %}
              <a href="{{ page.slidesurl }}">Slides</a>
            {% endif %}

            {% if page.videourl %}
              {% if page.paperurl or page.fulltexturl or page.slidesurl %} | {% endif %}
              <a href="{{ page.videourl }}">Video</a>
            {% endif %}

            {% if page.bibtex %}
              {% if page.paperurl or page.fulltexturl or page.slidesurl or page.videourl %} | {% endif %}
```

Note the BibTeX separator condition gains `or page.videourl` so it correctly adds ` | ` when only a video link precedes it.

- [ ] **Step 3: Build the site and verify the optrules page is unaffected**

`2021-optrules.md` has no `videourl` field, so no Video link should appear there.

```bash
bundle exec jekyll build 2>&1 | tail -5
grep -A2 "Slides" _site/publication/2021-optrules/index.html
```

Expected: build exits cleanly; the optrules page shows `Slides` with no `Video` link adjacent.

- [ ] **Step 4: Verify the BibTeX separator logic with a paper that has slides but no video**

```bash
grep -A5 "Slides\|BibTeX\|Video" _site/publication/2021-optrules/index.html | head -20
```

Expected: `Slides | BibTeX` appears with a single ` | ` separator and no `Video`.

- [ ] **Step 5: Commit**

```bash
git add _layouts/single.html
git commit -m "feat: add videourl field and Video link to publication page layout"
```

---

### Task 2: Add videourl and SlidesLive embed to bayesbeatscv

**Files:**
- Modify: `_publications/2023-bayesbeatscv.md`

- [ ] **Step 1: Add `videourl` to the front matter**

Open `_publications/2023-bayesbeatscv.md`. In the front matter (between the `---` delimiters), add after `slidesurl`:

```yaml
videourl: 'https://slideslive.com/39010083/bayes-beats-cross-validation-efficient-and-accurate-ridge-regression-via-expectation-maximization'
```

- [ ] **Step 2: Add the SlidesLive iframe to the body**

The body currently starts with `**Abstract:**`. Insert the iframe above it:

```html
<iframe src="https://slideslive.com/embed/presentation/39010083"
        width="100%" height="400" frameborder="0"
        allowfullscreen scrolling="no" allow="autoplay; encrypted-media">
</iframe>

**Abstract:**
```

- [ ] **Step 3: Build and verify the Video link appears on the bayesbeatscv page**

```bash
bundle exec jekyll build 2>&1 | tail -5
grep "Video" _site/publication/2023-bayesbeatscv/index.html
```

Expected: build exits cleanly; output contains `<a href="https://slideslive.com/39010083/...">Video</a>`.

- [ ] **Step 4: Verify the iframe is present**

```bash
grep "slideslive.com/embed" _site/publication/2023-bayesbeatscv/index.html
```

Expected: `<iframe src="https://slideslive.com/embed/presentation/39010083" ...` appears in the output.

- [ ] **Step 5: Verify the full link row renders correctly**

```bash
grep -E "Paper|Slides|Video|BibTeX" _site/publication/2023-bayesbeatscv/index.html | head -5
```

Expected: the rendered HTML contains `Paper`, then `Slides` is absent (bayesbeatscv has no `slidesurl`), then `Video`, then `BibTeX` — each separated by ` | `.

- [ ] **Step 6: Commit**

```bash
git add _publications/2023-bayesbeatscv.md
git commit -m "feat: add SlidesLive video link and embed to bayesbeatscv publication"
```
