# parsnips.net

Personal technical blog built with Jekyll and hosted on GitHub Pages.

## Setup

This site uses Jekyll with Ruby 2.6.10. The required gems are installed locally to avoid permission issues.

### Prerequisites

- Ruby 2.6+ (currently using 2.6.10)
- Git

### Installation

1. Install Jekyll and required gems:
```bash
# Install gems to user directory
gem install --user-install bundler -v 2.4.22
gem install --user-install public_suffix -v 4.0.7
gem install --user-install addressable -v 2.7.0
gem install --user-install jekyll -v 4.2.2
gem install --user-install minima -v 2.5.1

# Add gem executables to PATH
export PATH="$HOME/.gem/ruby/2.6.0/bin:$PATH"
```

2. Clone the repository:
```bash
git clone git@github.com:parsnips/parsnips.github.io.git
cd parsnips.github.io
```

## Development

### Running Locally

```bash
# Add gems to PATH (add this to your shell profile for persistence)
export PATH="$HOME/.gem/ruby/2.6.0/bin:$PATH"

# Serve the site locally with auto-reload
jekyll serve

# Serve with drafts included
jekyll serve --drafts

# Serve on a specific port
jekyll serve --port 4000
```

The site will be available at `http://localhost:4000`

### Building the Site

```bash
# Build the static site
export PATH="$HOME/.gem/ruby/2.6.0/bin:$PATH"
jekyll build
```

Built files are generated in the `_site/` directory (ignored by git).

## Content Management

### Adding New Blog Posts

1. Create a new file in `_posts/` following the naming convention:
   ```
   YYYY-MM-DD-title-with-hyphens.md
   ```

2. Add front matter at the top:
   ```yaml
   ---
   layout: post
   title: "Your Post Title"
   date: YYYY-MM-DD
   categories: [optional, categories]
   tags: [optional, tags]
   ---
   ```

3. Write your content in Markdown below the front matter.

### Adding New Pages

1. Create a `.md` file in the root directory or create a folder with an `index.md`

2. Add front matter:
   ```yaml
   ---
   layout: page
   title: "Page Title"
   permalink: /page-url/
   ---
   ```

3. Write your content in Markdown.

### Working with Drafts

1. Create files in `_drafts/` without dates in filename:
   ```
   _drafts/my-draft-post.md
   ```

2. Serve with drafts to preview:
   ```bash
   jekyll serve --drafts
   ```

## Site Structure

- `_config.yml` - Site configuration
- `_posts/` - Blog posts
- `_drafts/` - Draft posts (not published)
- `_layouts/` - Custom HTML templates
- `_includes/` - Reusable template partials
- `assets/` - CSS, images, and other static files
- `index.md` - Homepage content
- `CNAME` - Custom domain configuration for GitHub Pages

## Customization

### Modifying Layouts

The site uses custom layouts that extend the Minima theme:

- `_layouts/default.html` - Base template
- `_layouts/home.html` - Homepage layout
- `_layouts/page.html` - Static pages
- `_layouts/post.html` - Blog posts

### Adding Custom CSS

Add custom styles to files in the `assets/` directory or modify the existing theme files.

### Includes

Reusable components are in `_includes/`:
- `header.html` - Site header
- `footer.html` - Site footer
- `sub-footer.html` - Additional footer content

## Deployment

The site is automatically deployed to GitHub Pages when you push to the `main` branch.

### Manual Deployment

```bash
# Commit your changes
git add .
git commit -m "Your commit message"

# Push to GitHub
git push origin main
```

GitHub Pages will automatically build and deploy the site within a few minutes.

## Domain Configuration

The site is configured to serve at `parsnips.net` via the `CNAME` file. DNS must be configured to point to GitHub Pages:

- **CNAME record**: `parsnips.net` → `parsnips.github.io`
- **Or A records**: `parsnips.net` → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`

## Troubleshooting

### Jekyll Not Found

If you get "command not found: jekyll", ensure the gem bin directory is in your PATH:

```bash
export PATH="$HOME/.gem/ruby/2.6.0/bin:$PATH"
```

Add this to your shell profile (`.bashrc`, `.zshrc`, etc.) to make it permanent.

### Theme Issues

If you encounter theme-related errors, ensure all required gems are installed:

```bash
gem install --user-install minima -v 2.5.1
gem install --user-install jekyll-feed
gem install --user-install jekyll-seo-tag
```

### Build Failures

Check the GitHub Pages build status in the repository's Actions tab or Pages settings if the site doesn't update after pushing changes.