# Operational Workflow Notes

This file contains dispatch and automation details for manual review before any
public release of the repository. It is intentionally separate from `AGENTS.md`
so the main project instructions stay focused on public-safe source and content
conventions.

## Project Dispatch Identity

- Public site: `https://blog.ristic.in.rs`

## Discord Dispatch Workflow

PyCodeBridge supports direct session commands and multi-agent dispatch:

- `@codex <prompt>`: send one implementation/writing task to Codex.
- `@claude <prompt>`: send one planning, review, or reasoning-heavy task to Claude.
- `@claude @codex <prompt>`: have Claude plan first, then Codex execute from that plan.
- `@codex @gemini <prompt>` or similar fan-out: compare multiple agent outputs on separate worker branches.

## Branch And Session Behavior

- First dispatch in a channel session creates a persistent `task/<repo>/<timestamp>` branch.
- Worker branches are created per agent and per dispatch.
- Later dispatches in the same channel session continue the same task branch.
- Close the task with `!c done`; prefer `!c done --pr` for this blog so Aleksandar can review before GitHub Pages publishes anything.
- Use `!c done --merge` only after explicit approval, because merging/pushing can publish the site.

## Recommended Blog Dispatch Prompts

- New draft: `@codex draft post about <topic>. Create _drafts/<slug>.md only; do not publish. Include front matter, tags, and a reminder to preview with bundle exec jekyll serve --drafts.`
- Review draft: `@claude review _drafts/<slug>.md for structure, technical accuracy, voice, links, tags, and Jekyll rendering. Do not publish.`
- Revise draft: `@codex update _drafts/<slug>.md based on this feedback: <notes>. Keep it unpublished.`
- Publish approved draft: `@codex publish the approved draft _drafts/<slug>.md as _posts/YYYY-MM-DD-<slug>.md. Set the final date, preserve permalink requirements, run bundle exec jekyll build, and report the result.`

## Useful PyCodeBridge Commands

- `!c status`: see the current session state.
- `!c changes`: summarize repo changes.
- `!c branch`: show the current branch.
- `!c git status`: inspect git state.
- `!c answer <text>`, `!c approve`, `!c deny`: respond when an agent asks for input.
- `!c done --pr`: push the task branch and open a draft PR for review.

## Dispatch-Safe Blog Workflow Reminder

When Aleksandar mentions Discord, PyCodeBridge, dispatch, `@codex`, `@claude`,
remind him of the dispatch-safe version of the blog
workflow: draft in `_drafts/`, preview with `bundle exec jekyll serve --drafts`,
publish only after approval, then prefer `!c done --pr`.
