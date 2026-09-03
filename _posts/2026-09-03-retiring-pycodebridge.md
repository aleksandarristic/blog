---
layout: post
title: "Retiring PyCodeBridge"
date: 2026-09-03 11:42:02 +0200
tags:
  - "rant"
  - "ai"
  - "agents"
  - "codex"
  - "claude"
  - "gemini"
  - "opinion"
  - "pycodebridge"
---

PyCodeBridge is done. Not paused, not in maintenance mode — done. I'm archiving it and writing up why.

For anyone who missed the first two posts: PyCodeBridge was a Discord bridge into local coding agents. Send a message from your phone, it runs against Codex, Claude Code, or Gemini CLI on your dev machine, and the results stream back into the channel. I built it so I could keep working from odd moments during the day, and I kept building it because picking which agent to use, per session, from my phone, seemed like a good idea at the time.

It worked fine. I'm retiring it anyway, because the reason I built it that way doesn't hold up anymore.

## The Bet

The whole thing was built around an assumption: that Codex, Claude, and Gemini would stay roughly competitive, and it'd be worth routing between them depending on the task. So I built an abstraction layer — backend selection per session, `!c agent claude`, `!c agent codex`, `!c agent gemini` — so I wouldn't be locked into one vendor's CLI.

That was a reasonable bet to make when I didn't know who'd come out ahead. But in practice, I kept reaching for the same two tools regardless of what the router let me choose.

## What Actually Happened

Anthropic and OpenAI pulled ahead. Every session where it mattered, I used Claude Code or Codex, and `!c agent gemini` mostly just sat there in the docs.

Claude Code and Codex also got good enough on their own that the router stopped earning its keep. They absorbed most of the workflows I was gluing together by hand. I ended up spending more time maintaining the bridge than actually needing it.

## Where Gemini Fit In

Gemini CLI was consistently the weakest of the three for me — not "different tradeoffs," just less reliable for stuff I'd trust running unsupervised against a real repo. I kept the backend wired in anyway, partly because "multi-agent" sounded more interesting than "two agents I use and one I keep around." That's on me, not really a knock on Google — they clearly have the infrastructure, it just hasn't translated into a coding agent I reach for yet.

## The Platforms Caught Up Too

The other thing PyCodeBridge solved was remote access — running an agent from my phone without SSHing into anything. Both vendors have since built that in natively. Claude Code's `/remote-control` is solid enough now for toilet coding. ChatGPT's Connect feature is arguably even better, though it wants more upfront prep — you set up a project ahead of time rather than just spinning up an ad-hoc chat the way I could `/rc` into one with Claude.

Either way, both add less overhead and burn fewer tokens than routing everything through my own bridge. That's really what settled it: the problem PyCodeBridge solved isn't a problem anymore, so there's nothing left to justify maintaining it.

## So I'm Retiring It

Maintaining a three-way abstraction stopped making sense when it was really a two-way one in practice. So: PyCodeBridge is retired. The repo stays up, but there's no 3.0 planned, and no "wait and see if Gemini catches up" grace period.

What replaces it is simple: I just use Claude Code and Codex directly, without a router I built and maintain myself sitting in the middle.

Building it taught me a lot — about session state, queueing, and what it actually takes to run a side project against a real machine. None of that was wasted. I just don't need the hedge anymore.

RIP PyCodeBridge.
