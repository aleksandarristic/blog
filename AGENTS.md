# Agent Instructions

## Project Overview

This repository is Aleksandar Ristic's personal tech/security blog,
published as `Aleksandar's Online Notes` at `blog.ristic.in.rs`.
It is a Jekyll site intended to be hosted with GitHub Pages on a custom
subdomain of the main site. The codebase is mostly Markdown content, Liquid
templates, static CSS, and image assets. Ruby is pinned with `mise.toml`.

Public project identity:

- GitHub repository: `aleksandarristic/blog`
- Public site: `https://blog.ristic.in.rs`

Treat this `AGENTS.md` file as the durable project memory for agent work in
this repository. When a task involves writing, reviewing, importing, or
publishing a blog post, read the writing workflow below and surface the
relevant reminder at the moment it helps Aleksandar make the next decision.

Operational dispatch notes, command syntax, and branch/session behavior live in
`workflow.md`. Read that file before using Discord/PyCodeBridge-driven workflows
or changing automation behavior.

## Source Map

- `_posts/`: published blog posts. Filenames must use Jekyll's `YYYY-MM-DD-slug.md` format.
- `_drafts/`: unpublished drafts. Do not publish draft content unless explicitly asked.
- `pages/`: standalone pages such as About, Archive, and Tags.
- `_layouts/` and `_includes/`: Liquid templates for page structure.
- `assets/css/main.css`: site styling.
- `assets/images/`: committed image assets used by posts/pages.
- `_data/tag_aliases.yml`: optional tag normalization data.
- `_site/` and `.jekyll-cache/`: generated local output/cache. Do not edit or commit generated changes from these directories.

## Local Setup

Use Ruby 3.4 via mise when available:

```bash
mise install
```

Install dependencies with Bundler:

```bash
bundle install
```

## Build And Verification

Run a production-style build before finishing changes that affect content, layouts, includes, CSS, or config:

```bash
bundle exec jekyll build
```

For local preview:

```bash
bundle exec jekyll serve
```

The local site normally serves at `http://127.0.0.1:4000/`.
There is no separate test suite in this repository; use the Jekyll build as the primary verification step.

## Writing Workflow

New posts may be drafted by an agent, then reviewed and approved by Aleksandar before publication.
When Aleksandar is working on a new post, proactively remind him of the relevant Jekyll step at the transition points:
draft creation, local review, approval for publishing, and final build verification.

Shortcuts:

- `draft post`: create or update an unpublished draft in `_drafts/`. Use this as the default for new writing unless Aleksandar explicitly asks for a published post. Draft filenames do not need a date.
- `preview drafts`: remind Aleksandar to run `bundle exec jekyll serve --drafts` and review the local site at `http://127.0.0.1:4000/`.
- `publish post`: move an approved draft into `_posts/` with a `YYYY-MM-DD-slug.md` filename and complete front matter, including the final `date`. Only do this after explicit approval.
- `review post`: edit for clarity, structure, technical accuracy, links, tags, and rendering while preserving Aleksandar's voice.
- `import post`: preserve public URL compatibility while cleaning only the markup needed for Jekyll rendering.

Do not publish a new post, change a post date, or alter a public permalink without explicit approval from Aleksandar.
When drafting, leave the post in a reviewable state: valid front matter, reasonable tags, working links/images where possible, and no generated `_site/` edits.

Jekyll draft review flow:

1. Draft in `_drafts/my-post-slug.md` with front matter such as `title`, `layout: post`, and `tags`.
2. Preview drafts locally with `bundle exec jekyll serve --drafts`.
3. Iterate on edits while the post remains in `_drafts/`.
4. After Aleksandar approves publication, move the file to `_posts/YYYY-MM-DD-my-post-slug.md`.
5. Add or finalize `date: YYYY-MM-DD HH:MM:SS +ZZZZ` in the post front matter.
6. Run `bundle exec jekyll build` before considering the post ready to push.

For imported posts, check whether the generated URL matches the old public URL.
If not, add an explicit `permalink:` before publishing.

## Dispatch Workflow

When a task mentions Discord, PyCodeBridge, agent dispatch, or channel-driven
automation, consult `workflow.md` before acting. Preserve the same publishing
discipline from the writing workflow: draft in `_drafts/`, preview with drafts
enabled, publish only after explicit approval, and use a review step before
anything that can update the public site.

## Content Conventions

- Posts use YAML front matter with at least `title`, `date`, `layout: post`, and `tags`.
- Keep existing public URLs stable. If a filename or slug change would alter an
  old URL, add an explicit `permalink:` in front matter.
- Treat import fidelity as important: preserve authorial voice, dates, titles,
  tags, image references, and old public URLs unless the requested task says
  otherwise.
- Tags are rendered directly from post front matter on `/tags/`; keep spelling and casing consistent.
- Keep blog images local. Store new post media under `assets/images/YYYY/MM/post-slug/` and reference it with root-relative paths such as `/assets/images/YYYY/MM/post-slug/hero.png`.
- Do not use externally hosted images or `data:image` embeds in posts unless
  Aleksandar explicitly asks for an exception. If importing a post with remote
  images, download/rehost them locally before considering the import complete.
- Do not place source assets under `_site/`.
- Existing imported posts may contain raw HTML. Clean it up only when it is part
  of the requested change or needed to fix rendering.

## Template And Style Conventions

- Keep Liquid templates simple and static-site friendly; avoid adding client-side JavaScript unless explicitly needed.
- Maintain the current compact, monospace, dark theme in `assets/css/main.css`.
- Use root-relative URLs consistently, matching the existing includes and layouts.
- Keep generated feed and sitemap support intact via `jekyll-feed` and `jekyll-sitemap`.

## Change Discipline

- Do not modify `CNAME`, `url`, `baseurl`, or permalink settings unless the
  custom-domain or URL-preservation task explicitly requires it.
- Do not commit raw imports, private backups, secrets, or local caches.
- Keep edits focused on source files and documentation. Ignore unrelated local changes unless they block the requested work.
