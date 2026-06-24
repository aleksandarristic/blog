---
title: "PyCodeBridge Grew Up: Agentic Coding from Discord"
date: 2026-06-22 11:10:18 +0000
layout: post
tags:
  - "ai"
  - "agents"
  - "agentic coding"
  - "codex"
  - "claude"
  - "gemini"
  - "discord"
  - "remote development"
  - "vibe coding"
---

 <span style="color: #d4d4d4; font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px;">Back in February I wrote about PyCodeBridge as a way to doom-vibe-code from wherever I happened to be: phone in hand, Discord open, local development machine doing the real work somewhere else.</span>

That version was fun. It was also a bit of a prototype in spirit: a chat bridge into Codex CLI sessions, with dreams of multiple transports and just enough command handling to make mobile coding feel possible.

Several months later, the project has become something more serious.

PyCodeBridge is now an agentic coding bridge. It still lets me drive local repo work from chat, but the shape is different: Discord is the supported transport, channels are mapped to repos with `code-<repo>`, Codex is just the default backend, and Claude Code or Gemini CLI can be selected per session when I want a different agent.

In other words, it moved from "Codex in chat" to "a Discord control plane for agentic coding on my own machine."

## The New Mental Model

The basic idea is still simple:

1.  A private Discord channel named `code-myrepo` maps to a local git repository under `code_root`.
2.  Messages and commands in that channel are routed to an agent session for that repo.
3.  The agent runs locally, with access to the actual working tree, local tooling, credentials, and git state I have configured.
4.  Results stream back into Discord.

The important change is that the "agent" is now an abstraction, not a synonym for one CLI.

The default backend is Codex. That remains the out-of-the-box path and the config section is still named `codex` for compatibility. But sessions can now use:

- Codex
- Claude Code
- Gemini CLI

The command is straightforward:

```
!c agent claude
!c agent gemini gemini-2.5-pro
!c agent codex gpt-5.4 medium
```

There are also helper commands:

```
!c agents
!c models
!c efforts
!c agent
!c model
!c effort
```

The no-argument versions show what the current session is actually using. That matters more than it sounds, because once you have multiple threads, sessions, models, and backends, guessing is a great way to confuse yourself.

## Discord Won

The original post talked about Discord, Telegram, and Slack. That is no longer true.

The code still keeps a transport-aware architecture internally, but the supported transport is Discord. I use Discord. Discord has the interaction model I wanted: channels, private repo rooms, threads, typing indicators, uploads, downloads, pinned messages, and direct messages for owner/admin workflows.

So the product decision became simple: stop pretending every transport matters equally and make the Discord experience solid.

That also means the old channel prefix changed. It is no longer `codex-myrepo`. It is:

```
code-myrepo
```

That name fits the project better now. A channel maps to code. The selected backend can be Codex, Claude, Gemini, or whatever gets added later.

## It Is Not Just Prompt Relay Anymore

The bridge now has a real command surface. You can still send prompts, but the useful part is that you can operate sessions without being at the machine.

Some examples:

```
!c start
!c resume fix the failing tests
!c status
!c ps
!c stop
!c interrupt
!c kill
!c logs
!c download path/to/file
!c git status
!c gh pr status
```

There are shortcuts for active-run interaction:

```
!s tighten the scope
!a yes, proceed
!y
!n
!w
!retry
```

Plain chat can also become useful in the right state. If exactly one session is running, a plain message can steer that active session instead of being queued as a new prompt. If an agent is waiting for input, a plain reply can go straight to stdin.

That makes the mobile workflow feel much less like operating a bot and much more like supervising a long-running coding process.

## Sessions, Threads, Queues, and Conflict Handling

Each repo channel has a default session. Discord threads also get isolated session scopes, so I can split work without stomping on the main channel state.

There is queueing per channel, run control, stale-session handling, and explicit conflict choices:

```
!continue
!new
!compact
```

The `compact` flow is one of the more useful quality-of-life changes. When a session is stale or I want a fresh run without throwing away all context, the bridge can summarize prior context and start a new session from that summary.

That is the kind of feature that only shows up after actually using this thing for messy, interrupted work.

## Security Became a First-Class Feature

Running local coding agents from chat is powerful, which is another way of saying it can be dangerous if treated casually.

The current bridge is much stricter than the early version:

- Discord repo channels must be private.
- Users must be allowlisted.
- The bot is locked to one configured guild.
- TOTP can be required for protected commands.
- GitHub CLI commands can have their own unlock scope.
- High-risk operations stay behind TOTP.
- Failed or replayed TOTP attempts are rate-limited.
- Audit logs can redact secrets.
- TOTP values are sanitized before audit writes.

There are also safer file-transfer rules now. Uploads are bounded by per-file size, total batch size, and file count. Saves use repo-local temporary files and symlink-aware finalization so an upload cannot casually overwrite something outside the repo.

That sounds boring until you remember this is a chat interface into a development machine. Boring is good here.

## Better Feedback During Long Runs

Long agent runs are where bridges like this either feel magical or feel broken.

PyCodeBridge now tries much harder to tell me what is happening:

- Heartbeats can include agent, model, and effort.
- Tool calls can be surfaced in the chat stream.
- Claude thinking blocks can be relayed when enabled.
- Streamed output is coalesced so Discord does not get spammed by tiny chunks.
- Successful runs with no assistant output get an explicit terminal notice.
- Final result events stop runaway heartbeats even if a CLI process lingers.
- Friendly errors catch common backend failures, like unsupported models or Claude usage limits.

The point is not to make the chat noisy. The point is to avoid the worst state: staring at Discord wondering whether anything is alive.

## Docker, Health Checks, and Operations

The project also grew the less glamorous pieces that make it easier to run:

- Docker and Compose support.
- A preflight wrapper.
- An update-and-redeploy script.
- Optional health endpoint.
- Public health binds blocked by default unless explicitly allowed.
- Reset-state helper.
- Runtime logs, per-session JSONL logs, archives, and audit artifacts.

The bridge can still run directly with Python, but the Docker path is much cleaner for a persistent bot.

## What I Actually Use It For

I do not think of PyCodeBridge as "coding on a phone." That phrase sounds like punishment.

I think of it as remote supervision for local agentic coding.

The useful moments are things like:

- starting an investigation while away from the keyboard;
- asking an agent to inspect logs or summarize a repo state;
- nudging an already-running session;
- checking whether a long run is stuck;
- downloading a generated file;
- stopping a bad run before it wastes more time;
- switching a session from one backend to another when a task fits a different agent better.

It is still a little ridiculous. That is part of the charm. But it is no longer just a weekend toy.

## The Current Shape

The short version:

- Supported transport: Discord.
- Channel naming: `code-<repo>`.
- Default backend: Codex.
- Additional backends: Claude Code and Gemini CLI.
- Repo mapping: channels map to local git repos under `code_root`.
- Security: allowlists, private channels, guild lock, TOTP, rate limits, redaction.
- Operations: Docker, health endpoint, logs, archives, reset/update scripts.
- Workflow: sessions, threads, queues, run control, uploads/downloads, git and GitHub helpers.

That is a lot more than "send a prompt to Codex from Discord."

It is now a small, opinionated control plane for letting agentic coding tools work on my machine while I steer them from wherever I happen to be.

Still doom-vibe-code friendly. Just a lot less reckless.

Project:

<a href="https://github.com/aleksandarristic/pycodebridge" data-href="https://github.com/aleksandarristic/pycodebridge" style="color: #3794ff; text-decoration: none;">https://github.com/aleksandarristic/pycodebridge</a>
