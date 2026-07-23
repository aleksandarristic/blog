---
layout: post
title: "Remind: A Reminder App for the Terminally Scatterbrained"
date: 2026-07-23 13:53:28 +0200
tags: [android, kotlin, productivity, tools, adhd]
---

The universe trends toward disorder. Stars burn out, empires crumble, and somewhere between "I should really take that" and "I have definitely already taken that," a small but structurally important fact evaporates from my head without so much as a forwarding address. Entropy doesn't care whether the thing you forgot was a rent payment or your own medication. It only cares to me.

<img class="app-icon" width="96" src="/assets/images/2026/07/remind-a-reminder-app-for-scatterbrains/app-icon.svg" alt="Remind app icon: a plain white bell on a dark teal square" />

I have owned several reminder apps. Every one of them fired exactly once, said its piece, and considered the matter closed — like a coworker who mentions something to you a single time in a hallway and walks off to enjoy the rest of their apparently very trusting life. I do not need politeness. Politeness is how things get forgotten. I needed something with the manners of a smoke alarm, so I built it.

It's called Remind, and its entire personality fits in one sentence: it notifies you, and if you don't tap Done, it notifies you again, and again, until you either do the thing or explicitly tell it to stop.

<figure class="app-shot">
  <img src="/assets/images/2026/07/remind-a-reminder-app-for-scatterbrains/checklist-group.png" alt="Remind's Today agenda showing a grouped checklist of three reminders due before work" />
  <figcaption>One notification, three reminders, zero competing vibrations.</figcaption>
</figure>

## The Nag Loop

The mechanism is embarrassingly small for something that's meaningfully improved my life. An exact alarm fires, a notification appears, and the alarm receiver reschedules itself a few minutes out *before it does anything else*. No background service quietly keeping a candle lit — the nag just plants the seed of the next nag and moves on, indifferent to whether I'm proud of it.

Tap **Done** and the chain breaks, no hard feelings. **Skip** lets that one occurrence go without comment. Ignore it long enough, snoozing your way through several rounds, and the app stops asking nicely: a reminder can be configured to auto-escalate into a full-screen, alarm-tier takeover — siren tone, insistent vibration, the whole production — once it decides you've had your chances. It doesn't monologue about disappointment first. It just gets louder, the way consequences generally do.

<figure class="app-shot">
  <img src="/assets/images/2026/07/remind-a-reminder-app-for-scatterbrains/escalate-to-alarm.png" alt="The reminder editor with Escalate to alarm enabled, set to trigger after 60 minutes overdue" />
  <figcaption>Sixty minutes of being ignored, then the tone changes.</figcaption>
</figure>

The whole thing exists because "missed a dose" is not an acceptable outcome — that's the real design pressure, even though the app knows nothing about pills, dosages, or biology, and never will, deliberately. It just treats *every* reminder with the seriousness normally reserved for the one that actually matters. "Take the recycling out" earns the same tenacious persistence as anything else. The universe doesn't rank my responsibilities either.

## The Boring, Important Parts

Android can decide not to tell you something, for reasons ranging from a revoked permission to a manufacturer's battery-saving mood. Remind checks its own ability to fire before trusting it, and falls back to an inexact alarm rather than silently doing nothing. A late reminder is a minor annoyance; a missing one is a small betrayal.

There's no analytics SDK, no account to create, nothing phoning home just to feel useful — with one quiet exception. If I lose the phone entirely, the OS backs up the reminder database itself, to the Google account already signed in, because "forgot to take my meds" is a bad failure mode and "dropped my phone in a lake and lost every reminder with it" is worse. It's scoped to just that database, nothing else, and restoring it onto a new phone reasons about stale pending reminders the same way waking up from a reboot does — no panicking about something that was due while the phone was underwater. Dismissing a notification, meanwhile, is still not the same as doing the thing; the only way to mark something done is to say so, out loud, on purpose. This app does not trust me. It is, on the available evidence, correct not to.

## Status

It has picked up a few conveniences along the way I didn't originally ask for and now can't live without — widgets, a choice of themes, a locked/private mode for reminders I'd rather my lock screen not narrate to the room. None of that was the point. The point was one small, stubborn piece of software that refuses to let me off the hook the way I let myself off the hook, and so far it's holding the line better than I do.

<figure class="app-shot">
  <img src="/assets/images/2026/07/remind-a-reminder-app-for-scatterbrains/style-picker.png" alt="The style picker showing four agenda density options: Standard, Compact Agenda, Soft Contrast, and Comfortable" />
  <figcaption>Four ways to arrange the same low-grade dread.</figcaption>
</figure>

Right now it has an audience of exactly one mildly scatterbrained developer, sideloaded onto his own phone like a piece of samizdat. That's likely to change — the plan is to open source it and possibly push it out through an app store or two, on the theory that if this particular flavor of stubbornness has kept me on top of my own life, it might do the same for someone else's. Until then, it remains the only reminder app that has never once given up on me first.
