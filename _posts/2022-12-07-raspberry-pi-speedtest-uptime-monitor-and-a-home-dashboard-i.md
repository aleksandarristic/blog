---
title: "Raspberry Pi - speedtest & uptime monitor, and a home dashboard - intro"
date: 2022-12-07 09:29:00 +0000
layout: post
tags:
  - "ad block"
  - "grafana"
  - "dns"
  - "rpi"
  - "monitor"
  - "influxdb"
  - "speedtest"
  - "pihole"
  - "raspberry pi"
  - "uptime"
---

A almost a year ago, I received an awesome birthday gift. It was a brand new raspberry pi 4 device! I immediately saw an opportunity to have an ad-free home network via Pihole, a dnsmasq based ad blocker. That part was easy thanks to many online instructions on how to set it up. But what to do next? Over the course of a few months, I was trying to find ways on how to add more functionality to the tiny device sitting next to my ISP's router.

This post-series is the result of my tinkering with rasperry pi os and a few go-to software solutions for monitoring my internet speed, uptime of internal and external network devices and servers, as well as a dashboard (or if you will - a home page) for my home computers.

The end result is this:

- All network devices use Pihole as their DNS in order to block ads
- Internet speed test is performed periodically using the official speedtest.net client
- Speed test results are saved in InfluxDB and are visualised using Grafana
- Uptime of both home devices and external servers is monitored using Uptime Kuma
- All of this is visible as a neat-looking dashboard created using Django (Python ❤️)

...
