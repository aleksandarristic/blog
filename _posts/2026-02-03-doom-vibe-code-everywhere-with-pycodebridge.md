---
title: "Doom-Vibe-Code Everywhere with PyCodeBridge"
date: 2026-02-03 17:01:00 +0000
layout: post
tags:
  - "vibe coding"
  - "codex"
  - "remote development"
  - "ai"
  - "llm"
comments:
  - author: "Aleksandar"
    date: 2026-02-27T10:50:45Z
    content: "UPDATE: since this post has been published, I've updated the code many times. Now it's a lot tighter, more secure, has a bit more bells & whistles. Oh, and I've dropped all but Discord. I like Discord."
---

Ever been stuck on a bus, in a toilet queue, or doom-scrolling on mobile and thought: *“Man, I really wish I could just hack on my code right now”?* Well, I did something about that. Meet **PyCodeBridge** — the bridge between your mobile chat apps and your development machine. It lets you drop into *vibe-coding* mode from apps like Discord, Telegram, or Slack and run your local Codex CLI sessions without actually being at your desk.

## What the Hell Is PyCodeBridge?

At its core, PyCodeBridge lets you communicate between a transport channel (like a Discord channel named `codex-myrepo`) and a local Codex CLI session running under the hood. Every channel becomes a stream into a **Codex session** tied to a local repository. You can send prompts, get responses, even manage jobs — from your phone, from the loo, from wherever.

Here’s the vibe:

- **Transport–Agnostic Routing:** Messages from chat apps are routed to your local Codex session dispatcher. Channels map to repos.
- **Multi-Session Support:** Each channel can have its own session with queueing and control.
- **Run Control:** Stop, kill, or quit sessions right from chat.
- **Upload/Download Support:** Because prompts aren’t the only thing worth sending.
- **Admin Modes:** If you want owner control via DMs, you can set that up too.

It’s basically your dev machine’s REPL on a leash you can smack from your phone.

## How It Works (Briefly)

1.  **Config:** Create a `config.yaml` using the supplied `config.example.yaml`.
2.  **Discord/Telegram bot:** Set your bot token and channels. Channels follow a regex like `^codex-([A-Za-z0-9._-]+)$`.
3.  **Run It:** Use the main Python module to start the bridge against your Codex CLI and watch as messages flow back and forth.
4.  **Chat + Code:** Send commands like `!c start mysession` or regular prompts to run code against your local repos.

Behind the scenes, transports declare capabilities (threading, uploads, downloads, typing), and PyCodeBridge handles the translation to/from the Codex CLI JSONL output. Every channel acts like a *remote REPL window* that happens to live inside your chat app.

## Why This Is Ridiculously Fun

- **No need to open Vim on your phone.**
- **You can literally doom-vibe-code while doom-scrolling.**
- **Supports Discord and Telegram natively, Slack scaffold ready.**
- **Multi-session means you don’t stomp on yourself when switching projects.**

In other words: you can finally justify those hours in the bathroom — *productive bathroom coding*, baby.

## Where to Get It and How to Use It

Detailed setup instructions, configuration references, and transport adapters are all in the project’s documentation on GitHub. The README walks through prerequisites, installation, config options, and examples for running the bridge. Check it out here:

👉 **[Check out PyCodeBridge on GitHub](https://github.com/aleksandarristic/pycodebridge?utm_source=chatgpt.com)**
