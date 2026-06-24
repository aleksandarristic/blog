---
layout: post
title: "SRB CERT Has No RSS Feed, So I Made One"
date: 2026-06-24 00:00:00 +0200
tags: [infosec, rss, python, srbcert, tools, scraping]
---

If you work in infosec in Serbia, you probably know [SRB CERT](https://www.cert.rs) — the national Computer Emergency Response Team. They publish security advisories, incident reports, and notifications on their site. Good stuff, worth following.

The problem: no RSS feed. No Atom feed. Nothing. You either remember to check the site manually, or you miss things.

I got tired of that, so I built [feedbot](https://github.com/aleksandarristic/feedbot).

## What it does

Feedbot is a small Python scraper that fetches the [SRB CERT notifications page](https://www.cert.rs/rs/obavestenja.html), pulls out each advisory (title, date, link), and generates a proper RSS XML file from it. That file gets served as a static asset, so anything that understands RSS — your feed reader, your Slack RSS bot, whatever — can consume it.

The core stack is minimal:

- `requests` + `BeautifulSoup4` for fetching and parsing the HTML
- `rfeed` for building the RSS XML
- A `sources.json` config that maps CSS selectors to feed fields
- A shell script and a cron job that ties it all together

The config for SRB CERT looks roughly like this — you point it at the page, tell it which HTML elements contain the title, date, and link, give it a regex for parsing Serbian month names, and that's it:

```json
{
  "id": "srbcert",
  "name": "SRB CERT Obaveštenja",
  "language": "sr",
  "page": "https://www.cert.rs/rs/obavestenja.html",
  "base_url": "https://www.cert.rs/rs/",
  "locators": {
    "item": { "tag": "div", "class": "preporuka" },
    "title": { "tag": "h3",   "class": "title" },
    "date":  { "tag": "span", "class": "date" },
    "link":  { "tag": "a",    "class": "date", "attr": "href" }
  }
}
```

## How it runs

A cron job calls the update script on a schedule. The script runs feedbot, which writes fresh XML files to a directory served by nginx. The feed is live at [ristic.in.rs/rss/srb_cert.xml](https://ristic.in.rs/rss/srb_cert.xml) if you want to subscribe without running anything yourself.

## Why bother?

Partly because I wanted the feed for myself. Partly because it felt wrong that a national CERT — an organization whose job is to help people stay on top of security threats — doesn't offer a machine-readable way to follow their own alerts.

An RSS feed is a solved problem from 2001. It takes almost no effort to publish one. The fact that you have to scrape a government security site to get this basic functionality in 2024 is a mild indictment of how seriously some institutions take their own communication.

## It's generic

Feedbot isn't SRB CERT-specific. The `sources.json` approach means you can point it at any site that has a consistent HTML structure. If there's another site you want a feed for — add a source config, run the script, done.

The repo is at [github.com/aleksandarristic/feedbot](https://github.com/aleksandarristic/feedbot). PRs welcome, especially for more source configurations.
