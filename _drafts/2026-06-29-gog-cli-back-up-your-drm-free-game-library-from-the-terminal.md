---
layout: post
title: "gog-cli: Back Up Your DRM-Free Game Library from the Terminal"
date: 2026-06-29 12:00:00 +0200
tags: [python, cli, gaming, gog, tools, backup, pypi]
---

GOG is one of the few places left where you actually own the games you buy. No launchers phoning home, no license servers that can go dark and take your library with them — just installers, yours to keep. That's the whole point.

The problem is that "yours to keep" only holds if you actually keep them. GOG's client handles downloads fine, but if you want a scriptable, auditable, terminal-friendly way to back up your library to a NAS or an external drive, you're on your own.

So I built [gog-cli](https://github.com/aleksandarristic/gog-cli).

## What It Does

`gog` is a Python CLI that talks to GOG's API and lets you manage your game library backups from the command line. The core loop is:

1. Authenticate once.
2. Refresh your local library cache.
3. List, filter, and plan what you want to back up.
4. Actually back it up.
5. Verify and sync later.

Nothing exotic, but everything works the way a CLI tool should: dry-runs before real downloads, explicit `--yes` flags instead of surprise side effects, resumable downloads, checksum verification when GOG provides them.

## The Workflow

```sh
gog auth login
gog refresh
gog list purchased
gog plan --destination /path/to/backups --all --storage --check-free-space
gog backup --destination /path/to/backups --all --yes
```

`gog refresh` fills a local cache. Everything else reads from that cache — no unnecessary API calls when you're just browsing your library.

`gog plan` is the one I reach for most. It shows you exactly what a backup run would download, how big it is, whether you have enough space, without actually touching anything. Run it, sanity-check the output, then fire off the real backup.

## Filtering

The list and plan commands have more filtering options than I probably needed to write, but they're all useful once you have a large library:

```sh
gog list purchased --platform linux
gog list purchased --genre strategy --year 1998..2005
gog list purchased --search witcher
gog plan --destination /backups --all --platform windows --language en --storage
```

Year ranges, genre filters, fuzzy title search, platform selection. Unknown metadata is excluded by default; use `--include-unknown-year` or `--include-unknown-genre` if you want those rows.

## Batch Selection

For curated backup sets, you can put selectors in a plain text file:

```text
# first NAS batch
witcher_3
cyberpunk_2077
123456789
```

Lines starting with `#` and blank lines are ignored. Then:

```sh
gog plan --destination /backups --games-from games.txt --storage
gog backup --destination /backups --games-from games.txt --yes
```

`--games-from` is repeatable. Selectors can be product IDs, slugs, or titles. You can also mix `--game` flags and `--games-from` files in the same command.

## aria2c Support

The built-in downloader is fine. If you want parallel downloads and more speed, install `aria2c` and pass `--downloader aria2c`:

```sh
gog backup --destination /backups --games-from games.txt --downloader aria2c --yes
```

## Why Not Just Use the GOG Client?

The GOG Galaxy client works. But it's a GUI, it's not scriptable, and it doesn't make it easy to maintain organized, versioned backups with metadata you can inspect or diff later.

`gog-cli` is for the kind of person who wants a cron job that checks for library updates, plans the delta, and backs up new installers to a NAS — without clicking through anything. If that's not you, Galaxy is fine.

## Install

Requires Python 3.12+.

```sh
pip install gog-cli
```

Or from source:

```sh
pip install git+https://github.com/aleksandarristic/gog-cli.git
```

The project is at [github.com/aleksandarristic/gog-cli](https://github.com/aleksandarristic/gog-cli). It's MIT licensed, and currently at 0.2.1.
