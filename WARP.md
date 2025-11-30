# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Repository overview

This repo hosts the source for the "Stick Shift" blog, implemented as a Jekyll site using the Millennial theme.

Key pieces:
- `Gemfile` + `millennial.gemspec`: define the Jekyll theme gem and its runtime dependencies (`jekyll`, `jekyll-feed`, `jekyll-paginate`, `jekyll-sitemap`, `jekyll-seo-tag`).
- `_config.yml`: Jekyll site configuration (markdown engine, highlighter, permalink style, pagination size, plugin list, site title/description/author).
- `_data/settings.yml`: navigation menu, social links, and various text labels used across layouts.
- `_layouts/`: page skeletons for the home page, posts, static pages, and category listing pages.
- `_includes/`: reusable partials for header, footer, featured posts, post metadata, related posts, Disqus, Google Analytics, etc.
- `_sass/` and `assets/css/main.scss`: SCSS partials and entrypoint for site styling.
- `_posts/`: dated markdown posts (primary content). Posts use front matter (`layout: post`, `categories`, `tags`, `image`, `hidden`, etc.).
- `pages/`: category index pages (e.g. `stories`, `notes`, `logs`) that use `layout: category` and a `category` key to filter posts.
- `_archive/`: older or archived posts that are not part of the active post collection.
- `index.html`: home page using the `home` layout with pagination over `paginator.posts`.
- `404.md` / `404.html`: custom 404 handling.
- `.github/workflows/*.yml`: GitHub Actions workflows to build and deploy the Jekyll site to GitHub Pages.

The layouts and includes rely heavily on `site.data.settings` and `site.github.url` (provided by GitHub Pages' Jekyll environment) to build URLs, menus, and social icons.

## Running the site locally

This is a Ruby/Jekyll project managed with Bundler.

### Install dependencies

From the repo root:

```bash path=null start=null
bundle install
```

### Build the site

To build the static site into `_site/` using the same command as CI:

```bash path=null start=null
JEKYLL_ENV=production bundle exec jekyll build
```

This mirrors the `bundle exec jekyll build` step used in `.github/workflows/jekyll.yml`.

### Serve locally for development

To run a local development server with automatic regeneration:

```bash path=null start=null
bundle exec jekyll serve
```

You can add flags like `--livereload` if desired.

There are no repository-specific automated tests or linters configured; the primary correctness check is that `bundle exec jekyll build` completes without errors.

## Build and deployment pipeline

GitHub Actions workflows under `.github/workflows/` define the build and deploy process:
- `jekyll.yml`: on pushes to `main`, checks out the repo, installs Ruby 3.1 with Bundler caching, runs `bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"` with `JEKYLL_ENV=production`, uploads the `_site` artifact, and deploys to GitHub Pages.
- `github-pages.yml`: an alternative workflow using `actions/jekyll-build-pages@v1` and `actions/deploy-pages@v2` for Pages.

As a result, deployment is automatic on push to `main`; no manual deploy scripts live in this repo.

## Content model

Posts and pages use standard Jekyll front matter with some conventions specific to this site:
- Posts in `_posts/` typically include:
  - `layout: post`
  - `categories`: used for high-level grouping and category pages.
  - `tags`: additional free-form labels.
  - `image`: filename under `assets/img/` used as a featured image or background image.
  - `hidden`: a string flag (e.g. `"true"` or `"false"`) that can be used to control visibility in templates.
- Category pages in `pages/` (e.g. `stories.md`, `notes.md`, `logs.md`) specify:
  - `layout: category`
  - `category`: the category name to filter on.
  - `permalink`: the URL path for the category listing.

The `home` layout paginates over `paginator.posts` and renders each entry using the `featured-post.html` include, which uses post metadata and the `image` property to create a card-like hero block.

The `post` layout uses includes for:
- `post-date.html` to show the date (unless `site.hide_post_date` is set).
- `post-share.html` for sharing links (if not hidden).
- `related-posts.html` and `disqus.html` based on configuration flags from `_config.yml` and `_data/settings.yml`.

Navigation is driven entirely by `_data/settings.yml`:
- `menu`: defines the header links (mapped to pages like `/stories`, `/notes`, `/fyi`, etc.).
- `social`: Font Awesome icons and external URLs.

## Theming and layout structure

Styling is organized around SCSS partials in `_sass/` and compiled via `assets/css/main.scss`. Layout hierarchy is:
- `default.html`: base chrome (header, footer, includes, CSS/JS).
- `home.html`: extends `default`, renders paginated featured posts.
- `post.html`: extends `default`, renders a single blog post with optional hero image and sharing/related blocks.
- `page.html`: extends `default`, used for simple content pages.
- `category.html`: extends `default`, iterates `site.posts` and includes `featured-post.html` for posts matching the current page's `category`.

Images used by posts live under `assets/img/` and are referenced by filename in post front matter. Cards and headers use `site.github.url` to construct absolute paths to these assets.

## Authoring workflow notes

At least one post (`_posts/2024-07-08-Here-is-how-I-publish-to-StickShift.md`) documents an external authoring workflow:
- Content is drafted in Obsidian as markdown.
- A PowerShell script `..\.scripts\pandocpub.ps1` is used to convert and publish drafts into the `_posts` folder, handling filename conventions and embedded images.

This script is referenced but not included in the repo. When generating or modifying posts programmatically within this repo, prefer to:
- Preserve existing front matter structure and keys.
- Maintain the date-based filename convention in `_posts/` (`YYYY-MM-DD-title-with-dashes.md`).