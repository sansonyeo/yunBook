# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A Jekyll static blog built on the **Mediumish** theme (WowThemes). Content is a Korean-language
affiliate-marketing blog ("Teddy의 선물 블로그") — each post recommends a Coupang product via
affiliate links. Deployed to GitHub Pages.

## Commands

Local development (Ruby/Bundler):
```
bundle install
bundle exec jekyll serve      # http://localhost:4000, live reload
```

Local development (Docker, no Ruby needed):
```
docker-compose up             # jekyll serve --force_polling on :4000
```

There is no test suite, linter, or build step beyond `jekyll build`. To verify a change,
run the server and load the page, or run `bundle exec jekyll build` and inspect `_site/`.

## Deployment

Pushing to the **`mtngirl`** branch triggers `.github/workflows/jekyll-gh-pages.yml`, which
builds with `actions/jekyll-build-pages` and deploys to GitHub Pages. Note `mtngirl` (not
`main`/`master`) is both the default branch and the deploy trigger — the workflow's `on.push.branches`
must match whatever branch you deploy from.

## Architecture

Standard Jekyll layering; the "big picture" is how the pieces connect:

- **`_config.yml`** is the control center. Key settings: `authors` map (posts reference an author
  by key, e.g. `author: teddy`, resolved in templates via `site.authors[post.author]`);
  pagination via **jekyll-paginate-v2** (`paginate: 12`); category archive pages auto-generated
  by **jekyll-archives** at `/category/:name/`. Feature toggles read by templates: `adsense`,
  `lazyimages`, `google_analytics`, `mailchimp-list` (all string `"enabled"`/`"disabled"` flags,
  not booleans).
- **`_layouts/`** — `default.html` is the shell; `post.html`, `page.html`, `archive.html`,
  `categories.html`, `tags.html` extend it. Category/tag archive pages are driven by the plugin
  + these layouts.
- **`_includes/`** — reusable partials. `postbox.html` / `featuredbox.html` render post cards
  (featured cards appear only when a post has `featured: true`); `star_rating*.html` render the
  `rating` front-matter as stars; `analytics.html`, `adsense-under-header.html`, `share.html`,
  `toc.html`, `search-lunr.html` are conditionally included based on the `_config.yml` flags above.
- **`_pages/`** — standalone pages (`about.md`, `categories.md`, `tags.md`), included in the build
  via `include: ["_pages"]` in `_config.yml`.
- **`_sass/`** — theme SCSS partials (`_stars.scss`, `_syntax.scss`) compiled through `assets/`.

## Adding a post

Posts live in `_posts/` named `YYYY-MM-DD-<slug>.md`. The existing posts follow a strict
front-matter contract that the card/archive templates depend on:

```yaml
---
layout: post
title:  "..."
author: teddy            # must be a key under `authors` in _config.yml
categories: [ ... ]      # drives jekyll-archives category pages
tags: [ ... ]
image: https://...       # thumbnail; used by postbox/featuredbox cards
description: "..."        # used by jekyll-seo-tag
# optional:
featured: true           # surfaces the post in the homepage Featured section
rating: 4                # renders star_rating includes
---
```

Permalinks are `/:title/` (set in `_config.yml`), so the post's URL derives from its `title`,
not its filename slug.

## Conventions worth knowing

- `image` values may be absolute URLs (Coupang CDN) or site-relative paths; templates branch on
  `contains "://"` to decide how to prefix `site.baseurl` — keep that in mind when templating images.
- Content and UI copy are Korean; preserve existing language and tone when editing posts.
- Posts include affiliate links and a disclosure line ("본 블로그는 파트너스 활동을 통해...").
  When generating new posts, keep the disclosure.
