# Repository Guidelines

## Project Structure & Module Organization
The site is a Jekyll profile page rooted at `_config.yml` and the default branch. Reusable snippets live under `_includes/`, while static imagery and generated metrics are stored in `assets/`, `charts/`, and `images/`. GitHub Action definitions sit in `.github/workflows/`; treat files such as `images/github-metrics.svg` as generated outputs updated by automation.

## Build, Test, and Development Commands
Install the minimal toolchain once with `gem install bundler jekyll` (Ruby ≥3.0 recommended). To preview locally, run `bundle install --path vendor/bundle` and `bundle exec jekyll serve --livereload`. For CI parity, invoke `bundle exec jekyll build` to render into `_site/` and catch Liquid or Markdown issues before pushing. Regenerate metrics by re-running the `Metrics` workflow from the Actions tab rather than editing artifacts by hand.

## Coding Style & Naming Conventions
Markdown sections should stay concise, using emoji sparingly and avoiding trailing spaces so the generated profile remains clean. Keep HTML fragments indented two spaces per level and prefer double quotes for attributes, matching existing `_includes/head-custom-google-analytics.html`. When adding assets, favor kebab-case filenames and reference them with relative paths to ensure GitHub Pages resolves them correctly.

## Testing Guidelines
Before opening a pull request, run `bundle exec jekyll build` and confirm it exits cleanly. Follow up with `bundle exec jekyll doctor` if you modify configuration or plugins. Validate structured data—such as `assets/github-followed-ranking.json`—with `jq empty assets/github-followed-ranking.json` to catch syntax errors. After site changes, spot-check the generated `_site/index.html` locally or via `jekyll serve` to confirm badges and embeds load.

## Commit & Pull Request Guidelines
Recent commits trend toward `Updated with Dev Metrics` or `Update images/... - [Skip GitHub Action]`; emulate that specificity by naming the primary artifact touched and noting automation skips when applicable. Group unrelated edits into separate commits. Pull requests should include a short summary, any related issue links, and before/after screenshots when visual sections (badges, charts) change. Mention required secrets (e.g., `GH_TOKEN`, `WAKATIME_API_KEY`) if your change depends on them so reviewers can confirm the workflows remain green.

## Automation & Secrets
The repository relies on three workflows: `main.yml` for metrics, `jekyll-gh-pages.yml` for deployments, and `followed-rank.yml` for ranking data. Avoid committing API keys; instead update them through repository secrets. If a workflow fails, inspect the corresponding run logs and rerun with updated credentials rather than force-pushing regenerated files.
