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

<div style="text-align: justify;">

Now that we've got that covered, let's talk about what's going to get covered by this blog post:

</div>

<div>

1.  Adding RSS application to your Slack instance.
2.  Adding feeds from various sources (websites, github...)
3.  ~~Adding feeds from websites that don't have sources.~~ (\<- there will be a separate blog post on this)

### Now, let's get started

</div>

To add the RSS app to your slack instance, navigate to the following URL:

<div>

<div style="text-align: center;">

<span style="font-family: courier;">https://\<your-slack-instance-name\>.slack.com/apps/manage</span>

</div>

Replace the <span style="font-family: courier;">**\<your-slack-instance-name\>**</span> with, well, your Slack instance name. In the search box at the top of the page type "rss" and click on the first result in the dropdown:

</div>

<div class="separator" style="clear: both; text-align: center;">

<a href="/assets/images/Screenshot_2022-12-22_at_13.02.11.png" style="margin-left: 1em; margin-right: 1em;"><img src="/assets/images/Screenshot_2022-12-22_at_13.02.11.png" data-border="0" data-original-height="354" data-original-width="746" width="320" height="152" /></a>

</div>

\

Next up, you get presented with the page of your RSS app, with a small dialog to add new RSS feeds:

<div style="text-align: justify;">

\

</div>

<div class="separator" style="clear: both; text-align: center;">

<a href="/assets/images/Screenshot_2022-12-22_at_13.23.07.png" style="margin-left: 1em; margin-right: 1em;"><img src="/assets/images/Screenshot_2022-12-22_at_13.23.07.png" data-border="0" data-original-height="429" data-original-width="1017" width="320" height="135" /></a>

</div>

It's a good idea to create a channel dedicated to feeds (or specific feed topics) before you start adding feeds. Now, go ahead and add Feed URLs and select a Slack channel for it. To start you off, try adding the Schneier on Security by Bruce Schneier, an excellent source of news from the cybersecurity world. Copy and paste this to your Feed URL field, select a Slack channel and click "Subscribe to this feed" button:

<span style="text-align: left;"><span style="font-family: courier;">**https://www.schneier.com/feed/atom/**</span></span><span style="font-family: courier;">  </span>

Once you add more feeds, your RSS app page will start looking like this:

<div class="separator" style="clear: both; text-align: left;">

\

</div>

<div class="separator" style="clear: both; text-align: center;">

<a href="/assets/images/Screenshot_2022-12-22_at_13.23.32.png" style="margin-left: 1em; margin-right: 1em;"><img src="/assets/images/Screenshot_2022-12-22_at_13.23.32.png" data-border="0" data-original-height="668" data-original-width="957" width="320" height="223" /></a>

</div>

<div class="separator" style="clear: both; text-align: center;">

\

</div>

Every time there is a new article published on any of the RSS feeds you had subscribed to, you will get a new message in the selected Slack channel that will look something like this: 

<div class="separator" style="clear: both; text-align: center;">

<a href="/assets/images/Screenshot_2022-12-22_at_13.23.55.png" style="margin-left: 1em; margin-right: 1em;"><img src="/assets/images/Screenshot_2022-12-22_at_13.23.55.png" data-border="0" data-original-height="486" data-original-width="650" width="320" height="239" /></a>

</div>

**And there you have it!** You now have your own personal feed aggregator for free! And you won't get bunch of spam news because - you are the admin and you get to select what to subscribe to\!

\

As a bonus, you can also use Slack to set up various service notifications by cloud providers and services you use or are interested in. For example, you can add Google Cloud (<span style="font-family: courier; font-size: x-small;">**https://status.cloud.google.com/en/feed.atom**</span>) or Github status updates (<span style="font-family: courier; font-size: x-small;">**https://www.githubstatus.com/history.atom**</span>) by adding their feeds to your RSS app. Their updates will look something like this:

<div style="text-align: justify;">

\

</div>

<div class="separator" style="clear: both; text-align: center;">

<a href="/assets/images/Screenshot_2022-12-22_at_13.24.15.png" style="margin-left: 1em; margin-right: 1em;"><img src="/assets/images/Screenshot_2022-12-22_at_13.24.15.png" data-border="0" data-original-height="262" data-original-width="646" width="320" height="130" /></a>

</div>

\

<div class="separator" style="clear: both; text-align: center;">

<a href="/assets/images/Screenshot_2022-12-22_at_13.24.29.png" style="margin-left: 1em; margin-right: 1em;"><img src="/assets/images/Screenshot_2022-12-22_at_13.24.29.png" data-border="0" data-original-height="88" data-original-width="417" width="320" height="68" /></a>

</div>

\

### Where to get feeds?

Well, everyone has their interests. You can easily create a custom feed of news relevant to you and your interests. But if you're into cybersecurity and related topics, I can help start you off with these feeds:

<div>

- 
- <a href="https://www.schneier.com/" rel="nofollow" target="_blank">Schneier on Security</a>: <span style="font-family: courier;">https://www.schneier.com/feed/atom/</span>
- <a href="https://krebsonsecurity.com/" rel="nofollow" target="_blank">Krebs on Security</a>: <span style="font-family: courier;">https://krebsonsecurity.com/feed/</span>
- <a href="https://packetstormsecurity.com/" rel="nofollow" target="_blank">Packet Storm</a>: <span style="font-family: courier;">https://rss.packetstormsecurity.com/</span>
- <a href="https://thehackernews.com/" rel="nofollow" target="_blank">The Hacker News</a>: <span style="font-family: courier;">https://feeds.feedburner.com/TheHackersNews</span>
- <a href="https://grahamcluley.com" rel="nofollow" target="_blank">Graham Cluley</a>: <span style="font-family: courier;">https://grahamcluley.com/feed/</span>
- <a href="https://security.googleblog.com/" rel="nofollow" target="_blank">Google Online Security Blog</a>: <span style="font-family: courier;">https://feeds.feedburner.com/GoogleOnlineSecurityBlog</span>
- <a href="https://www.bleepingcomputer.com/" rel="nofollow" target="_blank">BleepingComputer</a>: <span style="font-family: courier;">https://www.bleepingcomputer.com/feed/</span>
- <a href="https://threatpost.com/" rel="nofollow" target="_blank">Threatpost</a>: <span style="font-family: courier;">https://threatpost.com/feed/</span>
- <a href="https://www.darkreading.com" rel="nofollow" target="_blank">Dark Reading</a>: <span style="font-family: courier;">https://www.darkreading.com/rss.xml</span>
- <a href="https://www.troyhunt.com" rel="nofollow" target="_blank">Troy Hunt</a>: <span style="font-family: courier;">https://www.troyhunt.com/rss/</span>
- <a href="http://googleprojectzero.blogspot.com" rel="nofollow" target="_blank">Project Zero</a>: <span style="font-family: courier;">http://googleprojectzero.blogspot.com/feeds/posts/default</span>
- <a href="https://cloudseclist.com" rel="nofollow" target="_blank">CloudSecList</a>: <span style="font-family: courier;">https://cloudseclist.com/feed.xml</span>
- <a href="https://blog.intigriti.com/" rel="nofollow" target="_blank">Intigriti</a>: <span style="font-family: courier;">https://blog.intigriti.com/feed/</span>
- <a href="https://www.darknet.org.uk" rel="nofollow" target="_blank">Darknet</a>: <span style="font-family: courier;">https://www.darknet.org.uk/feed/</span>

</div>
