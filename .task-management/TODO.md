# TODO (Immediate)

Rules:
- Use stable task IDs in format `TASK-####`.
- IDs are immutable and never reused.
- Tasks promoted from `.task-management/BUGS.md` keep the same ID.
- When moving a task to backlog, keep the same ID.
- When completing a task, remove it from this file and append it to `.task-management/DONE.md`.
- When dropping a task, remove it from this file and append it to `.task-management/REMOVED.md`.

## Active tasks

### Site features

- TASK-0001 - Add blog comments using a static-site-friendly provider.
  - Context: Aleksandar asked how to add comments to the Jekyll blog and whether Disqus-style services are still common. Recommendation was to prefer Giscus for this tech/security blog because it uses GitHub Discussions, keeps moderation in GitHub, and fits the likely audience better than Disqus.
  - Goal: Add optional comments to post pages without changing existing post URLs or requiring generated `_site/` edits.
  - Scope:
    - Decide on provider, likely Giscus unless Aleksandar chooses otherwise.
    - Enable/configure the required GitHub-side setup, such as Discussions and the Giscus app, outside the repo as needed.
    - Add a comments include, wire it into the post layout, and gate it with front matter or a site config option.
    - Keep the implementation static-site friendly and compatible with GitHub Pages.
    - Document how to enable/disable comments per post.
  - Acceptance criteria:
    - Comments render on posts where enabled.
    - Comments can be disabled globally or per post.
    - `bundle exec jekyll build` passes in an environment with Ruby/Bundler/Jekyll available.
    - No generated `_site/` changes are committed.
