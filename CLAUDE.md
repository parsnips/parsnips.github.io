# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Jekyll-based static site for parsnips.net, a personal technical blog. The site uses the Minima theme and follows standard Jekyll conventions for structure and organization.

## Key Commands

Since this is a Jekyll site without a Gemfile, Jekyll commands are run directly:

```bash
# Serve the site locally for development (if Jekyll is installed)
jekyll serve

# Build the site
jekyll build

# Serve with live reload and drafts
jekyll serve --drafts --livereload
```

## Architecture and Structure

### Content Organization
- `_posts/`: Blog posts in Markdown format following Jekyll naming convention (YYYY-MM-DD-title.md)
- `index.md`: Homepage content using the page layout
- `_config.yml`: Site configuration (theme: minima, title, description)

### Templates and Layouts
- `_layouts/`: Custom HTML layouts that override/extend the Minima theme
  - `default.html`: Base template with head, header, main content, and footer includes
  - `home.html`, `page.html`, `post.html`: Specialized layouts for different content types
- `_includes/`: Reusable template partials (header.html, footer.html, sub-footer.html)

### Static Assets
- `assets/`: Static files (CSS, images, etc.)
- `static/`: Additional static content
- `data/`, `i18n/`, `layouts/`: Additional directories (may be unused legacy from another generator)

## Content Creation

New blog posts should be created in `_posts/` with the format `YYYY-MM-DD-title.md` and include proper front matter:

```yaml
---
layout: post
title: "Your Post Title"
---
```

The site uses Jekyll's Liquid templating system and follows standard Jekyll conventions for front matter and content processing.