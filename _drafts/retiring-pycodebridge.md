---
layout: post
title: "Retiring PyCodeBridge: The War's Over and Nobody Needs a Bridge to a Winner"
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

PyCodeBridge is done. Not "on hiatus," not "in maintenance mode," not one of those quiet GitHub repos that technically still accepts issues nobody will ever triage. Done. Archived. I'm writing its obituary myself so nobody has to guess what happened.

For those who missed the first two installments: PyCodeBridge was a Discord bridge into local coding agents. Send a message from your phone, it runs against Codex, Claude Code, or Gemini CLI on your actual dev machine, results stream back into the channel. I built it because I wanted to keep working from a bus seat or a toilet queue, and I kept building it because the idea of picking *which* agent to point at a problem, per session, from my phone, felt like the smart bet.

It was the wrong bet. Not because the bridge didn't work — it worked great, right up until the premise underneath it stopped making sense.

## The Bet I Made

The whole architecture was built around a lie I told myself politely: that this was going to stay a real three-horse race. Codex, Claude, Gemini, all roughly competitive, all worth routing between depending on the task. So I built an abstraction layer. Backend selection per session. `!c agent claude`, `!c agent codex`, `!c agent gemini`. A transport-agnostic core so I wouldn't be married to any one vendor's CLI.

That's a reasonable thing to build if you genuinely don't know who's going to win. I didn't know. I hedged. Hedging felt responsible.

It wasn't. It was just work I did so I could avoid admitting what was already obvious every time I actually used the thing.

## What Actually Happened

Anthropic and OpenAI won. Not "are winning," not "have a lead" — won, in the sense that every session where it actually mattered, I reached for Claude Code or Codex, and the `!c agent gemini` command sat in the changelog looking decorative. I kept it in the docs because removing it felt like admitting something. Software should occasionally admit things.

Claude Code and Codex CLI got good enough, fast enough, that the entire reason to run a personal router in front of them started evaporating. The tools themselves absorbed the workflows I was gluing together by hand. Every quarter I spent less time writing bridge logic and more time just being a guy who owns a bridge nobody drives across, admiring the engineering of it.

That's the tell. When you're proud of the plumbing and nobody's using the faucet, the plumbing isn't the product anymore. It's a hobby wearing a README.

## Google Isn't Even in the Room

I want to be precise about this because I don't say it lightly: Gemini CLI, as an agent I'd trust to touch a real repository unsupervised, was consistently the weakest of the three. Not "different tradeoffs." Not "better at some things, worse at others" — the kind of even-handed sentence you write when you're being diplomatic about a product a friend built. Worse. Confidently wrong in ways that took longer to notice than the wrongness of a model that just says "I don't know." That's the expensive kind of dumb — the kind that looks like progress while quietly making more work for you downstream.

I kept the backend wired in anyway, because "multi-agent" sounded better in a blog post title than "two agents I actually trust and one I keep around out of some misplaced sense of fairness." That's on me, not on the code.

Google has infrastructure nobody else on earth has and somehow keeps shipping the coding agent equivalent of a very confident intern. I don't know what happens internally that turns that much compute and that much data into something I still don't trust to `rm` the right file. Not my problem to solve. Just not a horse I need a routing layer to bet on anymore.

## So I'm Killing It

There's no dignified way to keep maintaining a three-way abstraction when it's actually a one-and-a-half-way abstraction wearing a costume. Every `if backend == "gemini"` branch I write from here on is a lie about how I actually work, and I'm tired of maintaining fiction in a language that's supposed to be honest by construction.

So: PyCodeBridge is retired. The repo stays up because deleting history is a different kind of cowardice, but there won't be a PyCodeBridge 3.0, no fresh backend abstraction, no "wait for the next model release, maybe Gemini turns it around" grace period. I'm done building infrastructure to keep my options open on a race that's already been called.

What replaces it, for me, is embarrassingly simple: I just use Claude Code and Codex directly, wherever they're already good at meeting me — which, it turns out, is most places I actually need them, without a bridge I built and maintained myself standing in the middle taking a cut of the latency.

Building the router taught me plenty — about session state, about queueing, about how much operational hardening a "fun side project" needs before you'd trust it near a real machine. None of that was wasted. But the itch it was scratching, the fear of picking a lane too early, doesn't exist anymore. The lane picked itself. I just didn't want to admit that a Discord bot with a routing table isn't a personality, it's a sunk cost with good documentation.

RIP PyCodeBridge. You bridged me to exactly the place everyone else already was.
