# Repository Guidelines

## Project Structure & Module Organization

`README.md` is both the GitHub profile README and the GitHub Pages index, so changes must render well in both places. Blocks between `<!--START_SECTION:...-->` and `<!--END_SECTION:...-->` markers are rewritten by workflows; never edit inside them by hand. The site uses `jekyll-theme-minimal` configured in `_config.yml`, overridden by `_layouts/default.html` and `assets/css/style.scss`; keep `{% seo %}` and `{% include head-custom.html %}` (Google Analytics via `_includes/head-custom-google-analytics.html`) in the layout. `assets/` holds site resources and every generated output: `assets/github-metrics.svg` (`lowlighter/metrics`), `assets/bar_graph.png` (waka-readme-stats; path fixed by that action), and `assets/github-followed-ranking.json`. Treat these generated files as outputs, not sources.

## Build, Test, and Development Commands

There is no Gemfile; the site is built in CI by `actions/jekyll-build-pages` (the `github-pages` gem set). For a local preview, use a throwaway Gemfile containing `gem "github-pages", group: :jekyll_plugins` and run `bundle exec jekyll serve --livereload`; do not commit it. Regenerate metrics by re-running the `Metrics` workflow from the Actions tab rather than editing artifacts by hand.

## Coding Style & Naming Conventions

Markdown sections should stay concise, using emoji sparingly and avoiding trailing spaces so the generated profile remains clean. Keep HTML fragments indented two spaces per level and prefer double quotes for attributes, matching existing `_includes/head-custom-google-analytics.html`. When adding assets, favor kebab-case filenames and reference them with relative paths to ensure GitHub Pages resolves them correctly.

## Testing Guidelines

After pushing, confirm the `Deploy Jekyll with GitHub Pages dependencies preinstalled` run succeeds and spot-check the published page in light and dark mode and at phone width, including the mermaid follower chart rendered by the layout. Validate structured data—such as `assets/github-followed-ranking.json`—with `jq empty assets/github-followed-ranking.json` to catch syntax errors. After site changes, spot-check the generated `_site/index.html` locally or via `jekyll serve` to confirm badges and embeds load.

## Commit & Pull Request Guidelines

Recent commits trend toward `Updated with Dev Metrics` or `Update assets/github-metrics.svg - [Skip GitHub Action]`; emulate that specificity by naming the primary artifact touched and noting automation skips when applicable. Group unrelated edits into separate commits. Pull requests should include a short summary, any related issue links, and before/after screenshots when visual sections (badges, charts) change. Mention required secrets (e.g., `GH_TOKEN`, `WAKATIME_API_KEY`) if your change depends on them so reviewers can confirm the workflows remain green.

## Automation & Secrets

Workflows: `main.yml` (`Metrics`: Wakatime stats and `lowlighter/metrics`), `profile-content.yml` (bio table, follower history, followed ranking), and `jekyll-gh-pages.yml` (Pages deploy). The two README-writing workflows share the `profile-readme` concurrency group to avoid push races; keep it on any new workflow that commits. `.github/actions/waka-readme-stats/` is a local copy of `anmol098/waka-readme-stats` that patches the upstream image for anmol098/waka-readme-stats#679; once that PR is merged, delete the directory and switch `main.yml` back to `anmol098/waka-readme-stats@master`. Dependabot (`.github/dependabot.yml`) proposes action updates weekly; review major bumps against their changelogs. Avoid committing API keys; instead update them through repository secrets. If a workflow fails, inspect the corresponding run logs and rerun with updated credentials rather than force-pushing regenerated files.
