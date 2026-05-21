# CLAUDE.md — Portfolio (al-folio / Jekyll)

## Commands

```bash
# Local dev (recommended — Docker)
docker compose pull
docker compose up        # serves at http://localhost:8080

# Local dev (native Ruby/Jekyll)
bundle install
bundle exec jekyll serve # serves at http://localhost:4000
```

## Architecture

| Path | Purpose |
|------|---------|
| `_config.yml` | Site-wide settings (name, URL, theme, plugins) |
| `_data/socials.yml` | Social links (GitHub, GitLab, email, etc.) |
| `_data/repositories.yml` | GitHub repos to display on the repositories page |
| `_data/cv.yml` | CV / résumé structured data |
| `_pages/about.md` | Homepage content (bio, profile image) |
| `_pages/` | All site pages (CV, projects, news, profiles…) |
| `_posts/` | Blog posts |
| `_projects/` | Project cards |
| `_news/` | News/announcements items |
| `assets/` | Images, PDFs, JS, CSS |

## Deployment

Deployed to GitHub Pages at `https://brichardzafy.github.io`. Pushing to `main` triggers the `deploy.yml` GitHub Action, which builds and pushes to the `gh-pages` branch automatically (~4 min build time).

## Gotchas

- `baseurl` in `_config.yml` must stay as `/` for GitHub Pages personal site — do NOT set it to a subpath.
- Profile image goes in `assets/img/profile.jpg`.
- Bibliography entries live in `_bibliography/papers.bib` and are rendered by `jekyll-scholar`.
- `_config.yml` changes require a full server restart (not hot-reloaded).
