# Aleksandar's Online Notes

Personal blog covering infosec, tech, and the occasional off-topic ramble.
Live at **[blog.ristic.in.rs](https://blog.ristic.in.rs)**.

## Stack

- [Jekyll](https://jekyllrb.com/) 4.x — static site generator
- [GitHub Pages](https://pages.github.com/) — hosting, deployed via GitHub Actions on every push to `main`
- Ruby 3.4 (pinned via [mise](https://mise.jdx.dev/))
- Custom dark monospace theme (no frameworks)

## Local setup

```bash
mise install        # installs Ruby 3.4
bundle install      # installs Jekyll and plugins
```

## Running locally

```bash
bundle exec jekyll serve
```

Opens at `http://127.0.0.1:4000/`. To preview unpublished drafts:

```bash
bundle exec jekyll serve --drafts
```

## Writing a post

1. Create a draft in `_drafts/my-post-slug.md` with front matter:

   ```yaml
   ---
   layout: post
   title: "Post title"
   date: 2026-01-01 12:00:00 +0100
   tags: [tag1, tag2]
   ---
   ```

2. Preview locally with `--drafts`.
3. When ready to publish, move to `_posts/YYYY-MM-DD-my-post-slug.md`.
4. Push to `main` — GitHub Actions builds and deploys automatically.

## Structure

```
_posts/          published posts (YYYY-MM-DD-slug.md)
_drafts/         unpublished drafts
pages/           standalone pages (About, Archive, Tags)
_layouts/        Liquid page templates
_includes/       reusable template fragments (header, footer, head)
assets/css/      site stylesheet
assets/images/   post images (organised by YYYY/MM/post-slug/)
_data/           tag aliases and other structured data
```

## Deployment

Pushes to `main` trigger `.github/workflows/pages.yml`, which builds the site
with `JEKYLL_ENV=production` and deploys to GitHub Pages. No manual steps needed.
