# Repository Guidelines

## Project Structure & Module Organization
This Jekyll site keeps canonical content in `_posts/` (publish) and `_drafts/` (work in progress). Templates and partials live in `_layouts/` and `_includes/`, while `assets/` stores CSS, JS, and images; `data/` and `i18n/` feed structured content. Generated output in `_site/` is disposable, and root Markdown pages such as `index.md` and `about.md` should stay lightweight and delegate to posts.

## Build, Test, and Development Commands
Add the user gem bin directory before running anything: `export PATH="$HOME/.gem/ruby/2.6.0/bin:$PATH"`. `jekyll serve` boots the local preview at `http://localhost:4000`, with `--drafts` to surface `_drafts/`. `JEKYLL_ENV=production jekyll build` mirrors the GitHub Pages deploy and should run before pushes.

## Coding Style & Naming Conventions
Blog files follow `YYYY-MM-DD-title-with-hyphens.md` and front matter ordered `layout`, `title`, `date`, then optional metadata. Liquid and HTML use two-space indentation, `{%- %}` trimming when appropriate, and keep logic minimal inside includes. Place stylesheet changes in `assets/css/` and prefer descriptive class names over inline styling.

## Testing Guidelines
Treat `jekyll build` as the required regression check and resolve warnings or Liquid errors immediately. After touching layouts or data, run `jekyll serve` and click through key URLs (`/`, `/about/`, newest post) to confirm rendering. If html-proofer is installed locally, `bundle exec htmlproofer ./_site` is a helpful pre-PR smoke test.

## Commit & Pull Request Guidelines
Write concise, imperative commit subjects similar to existing history (e.g., `Configure Jekyll site for parsnips.net`) and keep related edits bundled. Document manual verification in each pull request body, call out affected URLs, and attach screenshots for visual changes. Link GitHub issues when applicable so Pages deployments and post reviews stay traceable.

## Configuration & Deployment Tips
Leave `_site/` untracked—GitHub Pages rebuilds from `main` using `_config.yml`. Preserve `CNAME` and review DNS whenever the domain configuration changes. When introducing new includes or data files, document any required front matter keys and update `_config.yml` comments so future contributors understand the dependency.
