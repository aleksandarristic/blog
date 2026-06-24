---
title: "Free personalized feed aggregator using Slack"
date: 2024-03-12 08:54:00 +0000
layout: post
tags:
  - "rss"
  - "news aggregator"
  - "cybersecurity news"
  - "rss feed aggregator"
  - "slack"
---

Over the course of the last few years, I've notices that it seems everyone and their mother uses Slack. At least for work and for a number of communities.

This blog post will show you how to create your own personalized feed aggregator using free Slack. I chose using Slack instead of Feedly or any other online or offline feed aggregator for the following reasons:

1.  I already use Slack; for work, for leisure, for a number of communities.
2.  You can easily share/add people to your Slack workspace.
3.  You can set up multiple channels and set up notifications for whichever you need.
4.  If you already are using Slack, you can always dedicate a channel or a few to your personal feed aggregator, without actually creating your own instance of Slack (but will need admin help to set everything up).

Now that we've got that covered, let's talk about what's going to get covered by this blog post:

1.  Adding RSS application to your Slack instance.
2.  Adding feeds from various sources (websites, github...)
3.  ~~Adding feeds from websites that don't have sources.~~ (<- there will be a separate blog post on this)

### Now, let's get started

To add the RSS app to your slack instance, navigate to the following URL:

```text
https://<your-slack-instance-name>.slack.com/apps/manage
```

Replace the **`<your-slack-instance-name>`** with, well, your Slack instance name. In the search box at the top of the page type "rss" and click on the first result in the dropdown:

[![Slack apps search showing RSS app](/assets/images/Screenshot_2022-12-22_at_13.02.11.png)](/assets/images/Screenshot_2022-12-22_at_13.02.11.png)

Next up, you get presented with the page of your RSS app, with a small dialog to add new RSS feeds:

[![Slack RSS app feed form](/assets/images/Screenshot_2022-12-22_at_13.23.07.png)](/assets/images/Screenshot_2022-12-22_at_13.23.07.png)

It's a good idea to create a channel dedicated to feeds (or specific feed topics) before you start adding feeds. Now, go ahead and add Feed URLs and select a Slack channel for it. To start you off, try adding the Schneier on Security by Bruce Schneier, an excellent source of news from the cybersecurity world. Copy and paste this to your Feed URL field, select a Slack channel and click "Subscribe to this feed" button:

```text
https://www.schneier.com/feed/atom/
```

Once you add more feeds, your RSS app page will start looking like this:

[![Slack RSS app with multiple feeds](/assets/images/Screenshot_2022-12-22_at_13.23.32.png)](/assets/images/Screenshot_2022-12-22_at_13.23.32.png)

Every time there is a new article published on any of the RSS feeds you had subscribed to, you will get a new message in the selected Slack channel that will look something like this:

[![RSS message in Slack](/assets/images/Screenshot_2022-12-22_at_13.23.55.png)](/assets/images/Screenshot_2022-12-22_at_13.23.55.png)

**And there you have it!** You now have your own personal feed aggregator for free! And you won't get bunch of spam news because - you are the admin and you get to select what to subscribe to!

As a bonus, you can also use Slack to set up various service notifications by cloud providers and services you use or are interested in. For example, you can add Google Cloud (`https://status.cloud.google.com/en/feed.atom`) or Github status updates (`https://www.githubstatus.com/history.atom`) by adding their feeds to your RSS app. Their updates will look something like this:

[![Google Cloud status update in Slack](/assets/images/Screenshot_2022-12-22_at_13.24.15.png)](/assets/images/Screenshot_2022-12-22_at_13.24.15.png)

[![GitHub status update in Slack](/assets/images/Screenshot_2022-12-22_at_13.24.29.png)](/assets/images/Screenshot_2022-12-22_at_13.24.29.png)

### Where to get feeds?

Well, everyone has their interests. You can easily create a custom feed of news relevant to you and your interests. But if you're into cybersecurity and related topics, I can help start you off with these feeds:

- [Schneier on Security](https://www.schneier.com/): `https://www.schneier.com/feed/atom/`
- [Krebs on Security](https://krebsonsecurity.com/): `https://krebsonsecurity.com/feed/`
- [Packet Storm](https://packetstormsecurity.com/): `https://rss.packetstormsecurity.com/`
- [The Hacker News](https://thehackernews.com/): `https://feeds.feedburner.com/TheHackersNews`
- [Graham Cluley](https://grahamcluley.com): `https://grahamcluley.com/feed/`
- [Google Online Security Blog](https://security.googleblog.com/): `https://feeds.feedburner.com/GoogleOnlineSecurityBlog`
- [BleepingComputer](https://www.bleepingcomputer.com/): `https://www.bleepingcomputer.com/feed/`
- [Threatpost](https://threatpost.com/): `https://threatpost.com/feed/`
- [Dark Reading](https://www.darkreading.com): `https://www.darkreading.com/rss.xml`
- [Troy Hunt](https://www.troyhunt.com): `https://www.troyhunt.com/rss/`
- [Project Zero](http://googleprojectzero.blogspot.com): `http://googleprojectzero.blogspot.com/feeds/posts/default`
- [CloudSecList](https://cloudseclist.com): `https://cloudseclist.com/feed.xml`
- [Intigriti](https://blog.intigriti.com/): `https://blog.intigriti.com/feed/`
- [Darknet](https://www.darknet.org.uk): `https://www.darknet.org.uk/feed/`
