---
layout: post
title: "I Built a Dashboard So My Raspberry Pi Could Tell on My Internet"
tags: [go, raspberry-pi, homelab, monitoring, influxdb, networking]
---

Every home network eventually develops folklore.

"The internet is slow after dinner."

"The Wi-Fi is fine, it must be the provider."

"That Raspberry Pi is probably overheating."

"It only drops when I actually need it."

None of those statements are useful by themselves. They are feelings wearing a trench coat. Maybe they are true. Maybe the ISP is having a bad day. Maybe the router is tired. Maybe the Pi is undervolting because the power supply was bought in a hurry three years ago and has been quietly judging everyone ever since.

I wanted fewer hunches and more receipts, so I built [gohome](https://github.com/aleksandarristic/gohome).

GoHome is a small self-hosted dashboard for a Raspberry Pi. It watches the internet connection, records speedtest and ping data, shows Raspberry Pi hardware metrics, and stores the time-series bits in InfluxDB. It also gives me a simple web UI for the stuff I care about when something feels wrong.

Not a cloud product. Not a "smart home platform." Not an observability stack pretending my apartment is a Kubernetes cluster.

Just a Pi on the network, taking notes.

## The Shape of It

The public dashboard is intentionally boring in the useful way. It shows the current state:

- speedtest status, download, upload, ping, jitter, server, and result link
- ping targets with average latency and packet loss
- Raspberry Pi temperature, load, CPU clock, voltage, memory, disk, uptime, and throttling state
- custom URL groups, because every home setup grows a little pile of local services

The admin panel is where the knobs live. From there I can configure schedules, speed thresholds, ping targets, InfluxDB settings, URL groups, users, and hardware/throttling options.

That split matters. The dashboard should be something I can glance at without logging in. The admin panel should be where I go when I want to change behavior. Those are different moods.

## Why Speedtest Is Not Enough

Speedtest is the obvious first feature because it gives the satisfying numbers. Download. Upload. Ping. A little badge saying everything is fine or not fine.

But speedtest alone is a blunt instrument.

If I run it once when things are bad, I learn that things were bad for one minute. If I run it once when things are good, I learn almost nothing. Home internet problems are often intermittent, and intermittent problems are where memory lies to you.

So GoHome can schedule speed tests and store the results. It can also skip the next scheduled run when I just ran one manually, because running tests back-to-back is a great way to turn monitoring into noise.

The thresholds are configurable too. "Good" depends on what I pay for, not on someone else's idea of broadband. If the connection is supposed to be 500 down and 250 up, I want the colors judged against that.

## Ping Is the Better Smoke Alarm

Ping targets are less glamorous, but they are often more useful.

A speed test tells me how fast the pipe looks under load. Ping tells me whether the path is alive and how ugly it feels. Packet loss and latency are the things users actually notice first: video calls glitch, SSH feels sticky, pages half-load, and everything gets blamed on Wi-Fi.

GoHome lets me define multiple ping targets and decide which ones show on the dashboard. I can watch the router, the ISP gateway, a public resolver, or some external service. If only the external target is bad, that tells a different story than if the local gateway is also bad.

Again, the point is not to build a NOC wall for one house. The point is to know where to start looking.

## The Pi Watches Itself

The more interesting part, at least to me, is that GoHome does not treat the Raspberry Pi as invisible infrastructure.

It collects Pi metrics with a deliberately fixed command set: `vcgencmd` for temperature, clocks, voltage, and throttling, plus `/proc` and filesystem stats for load, memory, disk, and uptime. The service avoids arbitrary command execution from UI input. That is a small design choice, but it is exactly the kind of boring security decision a home admin panel should make.

The throttling view is especially useful. Raspberry Pis can be dramatic about power and temperature, but not always loudly. `vcgencmd get_throttled` encodes the truth in a little hex value. GoHome turns that into something readable: whether throttling is active, what likely caused it, recent throttling windows, and how many events happened since boot.

That turns "the Pi feels weird" into "the Pi has been undervolting since yesterday afternoon."

Much better.

## Go Because It Should Be One Binary

This is the kind of project where Go makes sense.

I wanted one deployable binary, a normal systemd service, straightforward cross-compilation, and no ceremony. The build script stamps version and UTC build time, then the install script puts the binary in place, writes a systemd unit, sets up an environment file, creates a service user, and starts the service.

The Raspberry Pi deployment path is intentionally direct:

```bash
./build.sh v1.0.0
scp build/gohome-linux-armv7 pi@raspberrypi:/tmp/gohome
scp scripts/install_service.sh pi@raspberrypi:/tmp/
ssh pi@raspberrypi "chmod +x /tmp/install_service.sh /tmp/gohome && sudo /tmp/install_service.sh /tmp/gohome"
```

After that, configuration lives in `/etc/gohome.env`, runtime state lives under `/var/lib/gohome`, and logs are where systemd users expect them.

It is not fancy. That is the feature.

## The Dashboard Became a Small Home Console

The project started around network visibility, but the URL groups changed how it feels in daily use.

Once you already have a tiny local dashboard, it is natural to add links to the other little things in the house: router, Pi-hole, Grafana, NAS, internal tools, whatever else has appeared on the network. GoHome stores those as groups and entries, lets an admin reorder them, and keeps the public page useful as a launch pad.

This is how homelab tools earn their place. Not by being theoretically complete, but by sitting on the first tab you open when something needs attention.

## What I Like About It

GoHome is small enough that I can understand it, but complete enough that it solves the actual annoyance.

It has:

- a public dashboard and authenticated admin panel
- scheduled speedtest and ping collection
- InfluxDB storage for network measurements
- Raspberry Pi hardware and throttling metrics
- local URL groups
- systemd deployment
- tests around the parts that are easy to break

It also has the scars of a tool that got used: cache TTLs, lock files, skip-next-run behavior, trusted proxy config, session settings, drag-and-drop ordering, retention cleanup, and a pile of performance fixes in the UI.

Those are not headline features. They are the difference between "I made a dashboard" and "I still use this dashboard."

## The Point

I do not need my house to be smart.

I need it to be slightly less mysterious.

When the internet stutters, I want to know whether latency jumped, packet loss appeared, the speed test failed, the Pi throttled, or everything looks clean and the problem is somewhere else. GoHome gives me that without sending the data to someone else's platform or turning a simple Raspberry Pi into a lifestyle brand.

Just a Go binary, a small database, InfluxDB for measurements, and a dashboard that tells me what happened.

That is enough.

Project: [github.com/aleksandarristic/gohome](https://github.com/aleksandarristic/gohome)
