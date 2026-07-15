---
layout: post
title: "YeetTo: A Local URL Router for macOS and Windows"
tags: [rust, tauri, macos, windows, tools, url routing, productivity]
---

I open the same kinds of links all day, but I do not want all of them in the same place.

Work Jira belongs in a work browser profile. Personal YouTube does not. GitHub can mean several different things depending on the organization in the path. Spotify links should open Spotify. Zoom links should probably not become another forgotten browser tab. Sometimes I want a link to follow a rule, and sometimes I want to pick manually.

Operating systems mostly treat this as a default browser problem.

I wanted it to be a routing problem.

So I built [YeetTo](https://github.com/aleksandarristic/yeet-to).

## What It Does

YeetTo is a desktop URL router. It sits between the operating system and your browsers or native apps, then decides where a URL should go based on rules you control.

The basic idea is simple:

1. Set YeetTo as the handler for HTTP and HTTPS links.
2. Define destinations like "Chrome Work", "Brave Personal", "Firefox", "Spotify", or "Zoom".
3. Add rules that match hosts, paths, full URLs, or the source application.
4. Let matching links open in the right place automatically.
5. Ask manually when no rule exists, or when you force the picker.

Nothing leaves the machine. There is no service, no account, no sync backend, and no behavioral profiling trying to learn what I meant. The rules are local YAML, the routing engine is local Rust, and the desktop app is just the shell around it.

That is the whole point.

## The Problem

The default browser is too blunt.

If I click a work link in Slack, I usually want it in the work profile. If I click a music link, I want the app. If I click a random article, I want my normal browser. If I click the wrong Google account URL, I do not want to spend the next minute being gently punished by account switching.

Browser profiles helped, but they also created a new problem: the correct target depends on the link, not just the browser.

Tools in this space already exist, especially on macOS, but I wanted something that felt more like infrastructure than magic:

- readable configuration;
- predictable first-match-wins rules;
- a CLI that can explain decisions;
- no remote service;
- no hidden learning model;
- portable routing logic that is not married to one OS API.

That last part mattered more than I expected. YeetTo started as a macOS-shaped itch, but the thing I actually wanted was a small routing engine with desktop adapters around it.

## Rules Are Boring on Purpose

YeetTo configuration is YAML. A minimal setup looks like this:

```yaml
version: 1

fallback: brave-personal
learning_mode: true

destinations:
  brave-personal:
    type: browser
    browser: brave
    profile: Default

  chrome-work:
    type: browser
    browser: chrome
    profile: "Profile 2"

rules:
  - name: Work Jira
    match:
      host:
        exact: jira.company.com
    open_with: chrome-work

  - name: Company GitHub
    match:
      host:
        exact: github.com
      path:
        glob: /company/**
    open_with: chrome-work
```

Rules are evaluated top to bottom. The first match wins. That makes the behavior easy to reason about and easy to debug.

Matching can look at:

- hostname;
- path;
- full URL;
- source application.

The dimensions combine with AND. So a rule can say "GitHub links from Slack go to the work browser" without also catching every GitHub link I open from somewhere else.

There are exact matches, wildcard hosts, path prefixes, globs, and a restricted regex engine for cases where a glob is not enough. The regex support is intentionally boring too: no catastrophic-backtracking footguns on attacker-controlled URLs.

## Learning Mode

The best rule editor is the one I do not have to open for every new domain.

Learning Mode handles the unknown-link case:

```text
No rule matched
-> show the destination picker
-> open once, or remember this host/path for next time
```

That is not AI. It does not build a profile of my behavior. It just looks at the URL currently being opened and offers deterministic rule creation.

This is the difference between "the app guessed" and "I told it what to do once."

## Overrides

Rules are useful until the moment I want to ignore them.

YeetTo has a few escape hatches:

- hold the interactive modifier while opening a link to force the picker;
- arm "ask for next link" with a global shortcut;
- temporarily ask every time;
- temporarily force every link to a selected destination.

On macOS the interactive modifier is Option. On Windows it is Alt. The important behavior is the same: I can keep normal routing most of the time and still override it without editing configuration.

## Native Apps and Webmail

Browsers are not the only useful destination.

YeetTo can route to native applications too. A Spotify URL can become a Spotify app deep link. A Zoom URL can open Zoom. A custom application destination can use a URL template when the app has its own scheme.

It also supports generated webmail compose URLs, which matters for `mailto:` workflows. I do not want mail links to be another place where the operating system makes one global decision for every context.

The current model is intentionally explicit: configure the destination, let diagnostics tell you when something is missing, and keep the routing decision separate from the platform-specific launch mechanism.

## URL Cleanup

There is also opt-in URL cleanup.

YeetTo can remove common tracking parameters like `utm_*`, `fbclid`, `gclid`, and `msclkid`, plus custom parameter names or prefixes. Cleanup happens before matching and launching, so rules and destinations see the cleaner URL.

It is deliberately configurable. Some sites use query parameters for real state, and breaking those would be worse than leaving a tracking parameter alone. The defaults are conservative, and scoped exclusions are available when a host needs to be left untouched.

## The CLI

The desktop app uses the routing engine, but the engine is not trapped inside the desktop app.

There is a `yto` CLI:

```sh
yto config validate
yto test https://github.com/company/project
yto explain https://github.com/company/project
yto diagnostics
yto destinations list
```

The CLI is mostly there because I do not trust configuration I cannot inspect from a terminal. `yto explain` is the useful one: it tells me why a URL would route somewhere before I rely on the desktop handler doing it in the background.

That also made the internals cleaner. The router had to become a pure decision engine, not a pile of UI callbacks.

## The Architecture

YeetTo is split into three main pieces:

- `yeetto-core`: portable Rust routing engine and configuration layer;
- `yto`: command-line wrapper around the same engine;
- a Tauri desktop app with shared UI and platform adapters for macOS and Windows.

The core engine does not open browsers. It does not know about AppKit, Win32, Launch Services, registry keys, or profile folders. Given a compiled configuration, routing state, and a URL request, it returns a decision.

The desktop shell handles the messy OS parts: default-handler registration, browser/profile discovery, launching, tray/menu behavior, hotkeys, onboarding, diagnostics, and recovery flows.

That boundary made Windows support realistic. The Windows app is not a second product bolted on later; it uses the same router and shared Tauri UI, with a Windows adapter behind the platform facade.

## Platform Status

The current public release path is macOS. There are macOS DMGs on GitHub Releases, and the app is not notarized yet, so first launch still needs the usual right-click Open dance or quarantine removal.

Windows support is real but not something I want to oversell. The app builds and routes links on Windows through the same shared engine and UI. Browser discovery, profile-aware launching, Default Apps candidate registration, Alt override, diagnostics, and the main routing flows are implemented.

The missing part is release-quality Windows distribution: signed installers, updater artifacts, SmartScreen policy, and clean-machine validation on Windows 10 and Windows 11. Until that is done, Windows is a build-from-source and manual-validation path, not a polished download button.

That is a boring distinction, but an important one.

## Why I Built It This Way

The easy version of this app is a pile of platform-specific glue and a picker window.

That might have been fine for a weekend tool, but URL routing is the kind of thing that either becomes trustworthy or becomes annoying. If it is going to sit in the path of every clicked link, it needs to be predictable, inspectable, and recoverable.

So the project ended up with more unglamorous pieces than the first idea suggested:

- strict configuration validation;
- last-known-good config behavior;
- diagnostics for missing destinations;
- recovery windows when a rule points at something that no longer exists;
- onboarding instead of "go read the README";
- profile switching;
- rule filtering and grouping;
- testable matching logic;
- CI across macOS and Windows.

None of that is flashy. All of it matters when a tiny utility graduates from "interesting" to "I actually leave this running."

## Install

The project is at [github.com/aleksandarristic/yeet-to](https://github.com/aleksandarristic/yeet-to).

Current public releases are macOS DMGs from GitHub Releases. Windows can be built from source for now, while packaging and signing catch up.

The stack is Rust, Tauri 2, and a static HTML/CSS/JS frontend. The license is BSD 3-Clause.

YeetTo is still early, but it already solves the thing I wanted solved: links go where they belong, without turning my default browser into a junk drawer.
