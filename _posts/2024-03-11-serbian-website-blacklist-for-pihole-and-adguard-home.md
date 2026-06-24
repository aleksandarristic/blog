---
title: "Serbian Website Blacklist for PiHole and AdGuard Home"
date: 2024-03-11 17:31:00 +0000
layout: post
tags:
  - "blocklist"
  - "adguard"
  - "bezbedan balkan"
  - "adguard home"
  - "dns blocking"
  - "scam protection"
  - "fraud protection"
  - "blacklist"
  - "pihole"
---

In today's digital age, protecting oneself from online threats has become a necessity. With the ever-growing presence of adware, malware, fraud, and scams lurking on the internet, users need robust solutions to fortify their browsing experiences. Recognizing this need, I'm thrilled to introduce my latest creation: a Serbian website blacklist curated for PiHole and AdGuard Home users.

### Origin

This journey began with a simple yet profound realization: the internet landscape is rife with nefarious entities seeking to exploit unsuspecting users (including, say, my youngest family members). To counter this, I've become a user of AdGuard home running on my RaspberryPi in my home network. But there was somewhat of a gap in the sea of AdGuard blacklists out there - there are no (at least to my knowledge) blacklists dedicated to spammers and fraudsters operating in Serbia and Balkans.

Luckily, security and privacy aware users of the <a href="https://bezbedanbalkan.net/thread-1.html" target="_blank">Bezbedan Balkan forum</a> have started compiling all sorts of lists of nefarious websites targeted at internet users in Serbia and Balkans. I went on to compile their findings in a single list, using the PiHole/AdGuard home readable format.

If you've gone this far and only want to proceed to start using the list (and you know how to do it) - go ahead! You can copy and paste the following address to your PiHole or AdGuard home to add the new blocklist:

<div style="text-align: left;">

<span style="font-family: courier;"></span>

> <span style="font-size: large;">https://raw.githubusercontent.com/aleksandarristic/blacklist/main/lists/scam_hosts_srb.txt</span>

</div>

<span style="font-size: medium;">**Don't forget to visit the repository for details: <a href="https://github.com/aleksandarristic/blacklist" target="_blank">https://github.com/aleksandarristic/blacklist</a>**</span>

### Key Features and Benefits

<div>

1.  **Regular Updates**: Our blacklist is not static; it evolves with the ever-changing threat landscape. Updated at least weekly, to maintain protection against malicious entities. PRs on Github are welcome.
2.  **Coverage**: The blacklist contains different sections dedicated to Scams, Domain Typosquatting, Phishing and IoT related known malicious websites. This is by no means a "complete list", but it will significantly reduce your chances of getting scammed.
3.  **Compatibility**: The list has a usual format of website-per-line, with comment lines beginning with a "#" character. It's out of the box compatible with PiHole and AdGuard home (and possibly other tools out there).
4.  **Community-driven**: The list is compiled by active users of Bezbedan Balkan forum and is up-to-date with fraud and scam campaigns targeted at Serbian and Balkans internet users.
5.  **Tooling included**: The repository contains a small tool to easily update the list, along with keeping the existing sections, formatting, deduplication, simple substitution.

### Disclaimer

This repository and the blacklist is an effort completely done in free time. As many of you could assume, this is a cat and mouse game with the bad guys out there. I will try to update the list as soon as I can (sometimes many times in a single day), but it will always be behind the scammers and fraudsters. Be aware that this list may not protect you from scams, but it will certainly help reduce the chance for it.

### Obligatory shout-out to users of Bezbedan Balkan forum

</div>

If you haven't already heard about the <a href="https://bezbedanbalkan.net/thread-1.html" target="_blank">Bezbedan Balkan forum</a>, and you are interested in cybersecurity, osint, internet safety, privacy and other related topics - do go ahead and take a look at it. It's a valuable resource for everyone that wants to remain up-to-date in these topics regarding the whole Balkans. As a plus - there's some good people there that love this sort of work and they do it for free.

\

\
