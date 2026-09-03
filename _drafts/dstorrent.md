---
layout: post
title: "dstorrent: A Web Client for DownloadStation That Assumes Everything Will Fail"
tags: [synology, downloadstation, pwa, self-hosted, typescript, tailscale]
---

I run a Synology DS220j at home, and its DownloadStation web UI is the kind of interface that reminds you it was designed for a desktop browser on a LAN, not a phone on someone else's Wi-Fi. It works. It's also slow to load, awkward to use one-handed, and has opinions about connectivity that don't match how I actually check on downloads — which is mostly a quick glance from my phone, often from a network that isn't the home one.

So I built dstorrent: a PWA client for DownloadStation, installable on the home screen like a native app, that treats "the NAS is unreachable" and "my phone is offline" as normal operating conditions instead of error states.

## Two Resilience Layers, Not One

The core design decision is that neither the browser nor the backend ever assumes the thing behind it is up:

- `backend/` (Node + Fastify) is the only thing that talks to DSM. It polls DSM into its own SQLite cache, and `GET /api/tasks` is always served from that cache — instantly, even mid-DSM-outage. Mutating actions (pause, resume, delete, add) go through a durable queue with retry and backoff if DSM is unreachable, applied optimistically to the cache right away.
- `web/` (React + Vite) mirrors the same pattern one hop further out, using IndexedDB. If the *backend* is unreachable, the app still renders the last-synced state and queues actions locally to replay once the backend answers again.

Each layer only reasons about its own resilience — the backend doesn't know or care whether the browser is online, and the browser doesn't know or care whether DSM is. That separation was deliberate; collapsing them into one shared queue would make either one harder to reason about.

Polling is presence-gated on top of that: no connected client means no poll, not just a slower one, and the interval is a deliberately lazy 20 seconds rather than "as fresh as possible." An earlier version polled every 4 seconds regardless of whether anyone had the app open, which amounted to hammering a NAS that was already doing DownloadStation's job. A "↻ Sync" button forces an immediate poll on demand for when you actually want fresh numbers right now.

## The Fragile Part: Talking to DSM

Synology's DownloadStation API is the least trustworthy dependency in the stack, in ways that aren't documented anywhere obvious:

- API endpoints aren't all at the same cgi path, and it varies by DSM version — `SYNO.API.Auth` lives at `entry.cgi`, but `SYNO.DownloadStation.Task` is at `DownloadStation/task.cgi` on a real DS220j. Hardcoding either path was the root cause of an early bug where the backend confidently called the wrong endpoint for everything. It now discovers every API's real path and version at runtime via `SYNO.API.Info` and caches it per session.
- `pause`/`resume`/`delete` return `success: true` at the envelope level even when the actual per-task action failed. The real result is buried in `data: [{id, error}]` and has to be checked separately — trusting the envelope silently drops failures.
- A poll interval shorter than DSM's real round-trip time lets overlapping ticks race the client's shared session state, so the poll loop carries an in-flight guard to make sure a slow NAS response can't stack.

None of this is exotic engineering. It's the ordinary tax of building against an API that was written for Synology's own apps first and everyone else second.

## Boring, Important Parts

The SQLite layer had its own quiet failure mode: `CREATE TABLE IF NOT EXISTS` doesn't add new columns to an existing database, so a schema change silently broke every poll against a real NAS while passing every test against a fresh one. The fix was an additive migration path, and a standing reminder to verify schema changes against a real, previously-populated database, not just a clean one.

Also somewhat annoying: the prebuilt native binding for `better-sqlite3` intermittently SIGABRTs under Node 24's garbage collector. Switching to Node's built-in `node:sqlite` sidestepped both that and the need for native compilation on the deploy target.

Auth is a single shared account gated by password plus mandatory TOTP (or a single-use recovery code), with revocable, HMAC-signed sessions — because unlike DSM itself, this backend is meant to sit on a public subdomain, not just the home LAN.

## Deployment: Reaching a Private NAS From a Public Box

This part is also a decent worked example of a pattern worth stealing for anything else sitting on a home network: a small public server that needs to talk to a private device, without punching a hole in the home router for it.

The DS220j never gets a public IP, a forwarded port, or a DDNS hostname pointed at it. Instead:

- The Hetzner instance — the only thing anyone on the internet ever touches — runs `tailscale up` and joins the same tailnet as the NAS. That's the entire setup on the Synology side: DSM already had the Tailscale package installed and running, nothing DSM-specific was needed beyond that.
- Once both are on the tailnet, the backend just talks to `DSM_HOST` as a private address (e.g. `192.168.1.120`, or the NAS's Tailscale IP) as if it were on the same LAN. Docker Compose uses `network_mode: host` so the container inherits that route directly, without a sidecar container to bridge it in.
- Nothing about the DSM connection is exposed publicly — the tailnet is a WireGuard mesh between devices that have each authenticated to it, not a port anyone can scan into. The only public-facing surface is the backend's own HTTP API, fronted by host nginx and Certbot for TLS, with the backend and static web server both bound to `127.0.0.1` so nginx is the sole entry point.

The net effect: a NAS that has never once had a port forwarded on the router is reachable, securely, from a $5 VPS on the other side of the internet, and the only new thing installed to make that true was a userspace VPN client on each end. No VPN concentrator, no site-to-site tunnel config, no static IP requirement from the ISP.

The web app installs as a PWA on both Android Chrome and iOS Safari — no separate native build, no app store review, just "Add to Home Screen."

## Status

Three days, 28 commits, and it's already the thing I actually use to check on downloads instead of squinting at DSM's web UI on my phone. It's a single-user tool built for one NAS on one home network, and it's going to stay that shape — the point was never to generalize it, just to stop being annoyed every time I wanted to know if something had finished.
