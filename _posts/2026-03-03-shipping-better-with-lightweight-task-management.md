---
title: "Shipping Better With Lightweight Task Management"
date: 2026-03-03 13:18:00 +0000
layout: post
tags:
  - "ai"
  - "agents"
  - "task management"
  - "codex"
  - "claude"
---

I recently added a simple task-management layer to my AI CLI setup, and it has been a big quality-of-life upgrade.

The core idea is straightforward: keep work trackable without adding process overhead.

- Tasks use stable IDs (`0001`, `0002`, ...)
- Bugs use their own stable IDs (`BUG-0001`, `BUG-0002`, ...)
- Active files stay small (`TODO.md`, `BUGS.md`)
- History is preserved (`DONE.md`, `BUGS_DONE.md`, `REMOVED.md`)
- Progress is append-only (`progress_log.md`)

What I like most is that it works for both solo and agent-driven workflows. I can run tasks in sequence or parallel, and still keep clear visibility on what’s active, what’s done, and what was dropped.

I also added optional Discord notifications for agent progress. Secrets are handled safely by default .

The goal wasn’t to build another project-management system. It was to keep execution resumable, context small, and momentum high.

If you’re using coding agents, I highly recommend having a minimal, file-based task protocol like this. It scales surprisingly well.

Feel free to check it out in my repo dedicated to agent skills, policies and configurations: <https://github.com/aleksandarristic/ai-cli-config>
