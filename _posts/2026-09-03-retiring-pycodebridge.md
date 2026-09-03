---
layout: post
title: "Retiring PyCodeBridge"
date: 2026-09-03 11:42:02 +0200
tags:
  - "ai"
  - "agents"
  - "codex"
  - "claude"
  - "gemini"
  - "pycodebridge"
---

PyCodeBridge is retired. It was a Discord bridge that let me run Codex, Claude Code, or Gemini CLI on my dev machine from my phone, with results streamed back into the channel.

I built it assuming Codex, Claude, and Gemini would stay roughly competitive, so it was worth routing between them per session. That didn't hold up — in practice I always reached for Claude Code or Codex, and the Gemini backend mostly just sat there unused. Gemini CLI was also the least reliable of the three for anything touching a real repo unsupervised.

On top of that, both Anthropic and OpenAI shipped native remote access since I built this: Claude Code's `/remote-control` and ChatGPT's Connect feature (Connect needs more upfront setup — a project rather than an ad-hoc chat — but both work well). Both are lighter and cheaper on tokens than routing everything through my own bridge.

So there's nothing left for it to do. The repo stays up, but there's no PyCodeBridge 3.0. I just use Claude Code and Codex directly now.
