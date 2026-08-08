---
layout: post
title: "DiscoRSS: An RSS Bot That Outgrew Being a Discord Bot"
tags: [python, discord, telegram, rss, self-hosted, tools]
---

I already solved half of this problem once. A while back I wrote about [feedbot](/2026/06/24/srbcert-rss-feed.html), a scraper that generates an RSS feed for a site that stubbornly refuses to publish one. That solves the "how do I get a feed" problem. It says nothing about the "how do I actually see new items without babysitting a feed reader" problem, which is the one that actually costs me attention every day.

I wanted new items to just show up where I already am — a Discord channel — without me opening anything. So I built [DiscoRSS](https://github.com/aleksandarristic/discorss).

## What It Does

DiscoRSS polls RSS and Atom feeds, deduplicates entries against what it's already seen, and pushes only the new ones out to wherever you've told it to. It started as "an RSS bot for Discord," which is a fair description of the first working version, but the delivery side turned into its own thing:

- **Discord** — slash commands, per-channel subscriptions, embeds.
- **Telegram** — bot commands, chat-scoped subscriptions, HTML-formatted messages.
- **Webhooks** — plain JSON `POST` to any endpoint you configure.
- **A local CLI reader** — a cached, terminal-native reader for when I don't want any of the above and just want to read.

Same core underneath all four: fetch the feed, figure out what's new, hand it to whichever publisher owns that destination.

## Quickstart (Discord)

```bash
cp .env.example .env
```

```env
DISCORD_TOKEN=your_bot_token
DISCORD_GUILD_ID=your_server_id
DATABASE_PATH=/data/rssbot.sqlite3
```

```bash
mkdir -p data
docker compose up -d --build
docker compose logs -f
```

Then, from the server itself:

```text
/rss add url:https://example.com/feed.xml channel:#news
/rss list
/rss doctor
/rss test subscription_id:1
/rss remove subscription_id:1
/rss import file:subscriptions.opml channel:#news
/rss export
```

`/rss add` baselines the feed on add — you get everything from that point forward, not a backlog dump of the last five years of posts. `/rss doctor` gives a private per-feed health report, which turns out to be the command I actually reach for most, because "is this feed silently dead" is a much more common failure mode than "the bot crashed."

## It's Not Just a Discord Bot Anymore

Telegram works the same way, chat-scoped instead of channel-scoped:

```text
/rss_add https://example.com/feed.xml
/rss_list
/rss_test 1
/rss_remove 1
```

Webhooks skip the command surface entirely, since there's no chat to type commands into — they're configured straight from environment:

```env
WEBHOOK_SUBSCRIPTIONS=https://example.com/feed.xml=>https://hooks.example.com/rss
```

And the CLI reader is the one that surprised me by becoming genuinely useful on its own:

```bash
discorss-cli rss add https://example.com/feed.xml
discorss-cli fetch --all
discorss-cli show titles --limit 20
discorss-cli show item 42 --full
```

`fetch` hits the network and updates a local cache; `show` just reads that cache. Splitting those apart means I can read on a flight, cron the fetch separately, or run the whole thing on a laptop that spends half its life offline, without any of that touching Discord or Telegram credentials at all.

## Why the Core Doesn't Know Discord Exists

The temptation with a project like this is to let Discord leak into everything, because Discord was the first client and the one I cared about most. I didn't let that happen, mostly because I've built that mistake before and cleaning it up later is worse than just not making it.

```text
discorss/
├── core/            # config, db, feed parsing, OPML, security — no Discord dependency
├── integrations/
│   ├── discord/     # runtime, slash commands, publisher
│   ├── telegram/     # commands, formatting, publisher
│   ├── webhook/      # publisher only
│   ├── cli/          # local cache, renderer, commands
│   └── opml/         # service-level subscription backup/migration
└── main.py
```

The core package owns config, the database schema, feed fetching, a stable `entry_key` for dedup, OPML parsing, and a `Publisher` protocol. It has never imported anything Discord-shaped. Each integration owns its own runtime and formatting and talks to the core through that protocol. The CLI reader doesn't even import the Discord config module — it can run with zero Discord environment variables set, which was a deliberate constraint, not an accident.

That boundary is also why adding Telegram and webhooks didn't turn into a rewrite. They're new implementations of one interface, not new special cases bolted onto Discord-specific code.

## Feed URLs Are Attacker Input

Every subscribed feed URL and every webhook target is, from the server's perspective, a URL some admin typed in — which is a nice way of saying it's the kind of input that gets used for SSRF if you don't think about it. DiscoRSS checks that a feed host resolves to a public address before every fetch, and re-checks across manual redirects instead of trusting the first hop. Webhook targets get the same check before every `POST`.

I'll say the honest part out loud instead of pretending it's airtight: there's a known gap between that DNS check and the actual HTTP request — a rebinding attack could in principle swap the address in between. Closing that fully means a resolver that pins the validated IP for the request that follows it, which isn't implemented yet. Worth naming instead of quietly leaving out of the post.

The poll loop has its own rule that matters more than it sounds: an item is marked seen immediately after `publish()` succeeds, not batched at the end of a poll run. If a later item in the same batch fails to send — bad permissions, a rate limit, a network blip — the earlier ones that already went out don't get replayed on the next poll. Getting that ordering wrong is exactly how you end up double-posting the same three articles into a channel at 3am.

## OPML, Because I Don't Want to Retype Subscriptions

Both the service and the CLI reader support OPML import and export, which is the boring, standard, decades-old format that every feed reader already speaks. `/rss export` in Discord hands you back a private attachment; `discorss-cli rss export` writes a file straight to disk. Migrating between the two, or just keeping a backup that isn't a SQLite file I have to remember exists, is a two-command affair instead of a re-subscribe-to-everything afternoon.

## Project Status

DiscoRSS is self-hosted, Python, built on `uv`, deployed with Docker. Discord and Telegram are both live integrations today; webhooks and the CLI reader round out the sink list. The architecture doc in the repo explicitly leaves room for more integrations — Slack gets mentioned by name as the obvious next one — without forcing them into Discord-shaped assumptions the way the first prototype would have.

The actual daily use case ended up simpler than the feature list suggests: feeds I care about show up in a channel I already have open, without a scraper, a cron job I forget about, or a browser tab I never close. The rest — Telegram, webhooks, the CLI reader, OPML, the SSRF guard — is what happens when a weekend Discord bot has to survive contact with more than one way of actually wanting to read things.

Project: <https://github.com/aleksandarristic/discorss>
