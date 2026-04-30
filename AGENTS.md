# Assistant Debugging Notes

This repository is an al-folio/Jekyll GitHub Pages site. Treat this file as
public-safe operating guidance for future assistant sessions.

## Build and Deployment Context

- The main deployment workflow is `.github/workflows/deploy.yml`.
- The deployment build uses Ruby `3.2.2` via GitHub Actions.
- The production build step is:

  ```bash
  JEKYLL_ENV=production bundle exec jekyll build --lsi
  ```

- Deployment then purges unused CSS with:

  ```bash
  purgecss -c purgecss.config.js
  ```

- Prefer Docker or a Ruby `3.2.2` environment when reproducing deployment
  behavior locally.

## Local Production Build

Use Docker for the most portable local production build:

```bash
docker compose run --rm -e JEKYLL_ENV=production jekyll bundle exec jekyll build --lsi
```

For a non-Docker environment that matches the GitHub Actions flow:

```bash
pip3 install --upgrade jupyter
export JEKYLL_ENV=production
bundle install
bundle exec jekyll build --lsi
npm install
npx purgecss -c purgecss.config.js
```

Generated folders and dependency artifacts such as `_site/`, `.jekyll-cache/`,
`.sass-cache/`, `.bundle/`, `node_modules/`, and `Gemfile.lock` are ignored by
this repository.

## GitHub Actions Deployment Checklist

When deployment fails or the live site does not update, check the `Deploy site`
workflow first.

Review these steps in order:

1. `Setup Ruby`
2. `Update _config.yml`
3. `Install and Build`
4. `Purge unused CSS`
5. `Deploy`

If `Deploy site` succeeds, check the follow-up `Check for broken links on site`
workflow.

For build failures, inspect recent edits in common Jekyll content and template
areas:

- `_config.yml`
- `_news/`
- `_posts/`
- `_pages/`
- `_bibliography/papers.bib`
- `_includes/`
- `_layouts/`
- `assets/`

For deployed-site-only issues, verify that GitHub Pages is publishing from the
expected branch, that `CNAME` is correct, and that `_config.yml` keeps
`url: https://stoneboat.github.io` with an empty `baseurl`.

## Assistant Debug Template

Use this compact template when tracking a site bug or deployment issue:

```markdown
Symptom:

Changed files:

Reproduction command:

Output summary:

Relevant Actions workflow/run:

Suspected layer:

Fix attempted:

Verification result:
```

Common suspected layers include content, Liquid/layout, bibliography, assets,
CSS purge, and GitHub Pages configuration.

## Public-Safety Rule

Keep future edits to this file limited to public repository facts and reusable
debugging workflow. Do not add secrets, credentials, private notes, personal
environment details, or unpublished information.
