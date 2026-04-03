# External Video Integration Design

**Date:** 2026-04-03

## Goal

Support linking to externally hosted videos (e.g. SlidesLive) from publication pages, alongside the existing Paper / Slides / BibTeX links. Also apply to locally hosted videos where applicable.

## Scope

Touches only `_layouts/single.html` and `_publications/2023-bayesbeatscv.md`. The publications archive (`_includes/archive-single-publication.html`) is out of scope and left for a separate refactoring.

## Front Matter Field

Add `videourl` to publication front matter. A plain URL string — either an absolute external URL (e.g. SlidesLive) or a local path (e.g. `/files/2021-video.mp4`). No distinction is made in the data model; branching is handled in the layout.

## Layout Change (`_layouts/single.html`)

Add a `| Video` link in the existing `Paper | Slides | BibTeX` row, after Slides and before BibTeX, conditioned on `page.videourl`. The separator logic mirrors the existing pattern:

```liquid
{% if page.videourl %}
  {% if page.paperurl or page.fulltexturl or page.slidesurl %} | {% endif %}
  <a href="{{ page.videourl }}">Video</a>
{% endif %}
```

For local `.mp4` files the link opens the file directly in the browser. For external URLs it navigates to the external page. No auto-embedding is performed by the layout.

## Embedding (per-page, manual)

Embedding is authored manually in each publication's markdown body, as already done for `2021-optrules.md` with a `<video>` tag. For SlidesLive, an `<iframe>` is added to `2023-bayesbeatscv.md` above the abstract, sized with `width="100%"`. The `videourl` field does not drive embedding.

## Example: `2023-bayesbeatscv.md`

Front matter addition:
```yaml
videourl: 'https://slideslive.com/39010083/bayes-beats-cross-validation-efficient-and-accurate-ridge-regression-via-expectation-maximization'
```

Body addition (above abstract):
```html
<iframe src="https://slideslive.com/embed/presentation/39010083"
        width="100%" height="400" frameborder="0"
        allowfullscreen scrolling="no" allow="autoplay; encrypted-media">
</iframe>
```

## Potential Future Refactoring

- Add Video (and Paper/Slides) links to `archive-single-publication.html` for consistency across the publications list, likely by unifying paper entries in archive and other places.
- Consider a `_includes/video-embed.html` partial to avoid repeating iframe boilerplate across paper pages.
