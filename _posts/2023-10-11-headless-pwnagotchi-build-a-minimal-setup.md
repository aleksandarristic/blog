---
title: "Headless Pwnagotchi Build - A minimal setup"
date: 2023-10-11 09:36:00 +0000
layout: post
---

In case you are wondering what a Pwnagotchi is, I recommend reading <a href="https://www.evilsocket.net/2019/10/19/Weaponizing-and-Gamifying-AI-for-WiFi-Hacking-Presenting-Pwnagotchi-1-0-0/" rel="nofollow" target="_blank">this excellent article</a> by <a href="https://www.evilsocket.net/" rel="nofollow" target="_blank">Evilsocket</a>.

While there are some very comprehensive DIY tutorials for building a Pwnagotchi, I've noticed a lack of any minimal setup instructions out there. Considering that I had a Raspberry Pi Zero W laying around, I decided to go ahead and make the minimal build. The build is using only the Pi Zero W. There is no screen attached, no battery soldered to the Pi board, no additional antenna.

The goal of the mini-project is to build a Pwnagotchi and connect to it via Bluetooth from an iPhone. The most common problems I've seen out there are exactly Bluetooth related - how to reliably connect to your virtual hacking pet using BT. 

Final note before we begin - this blog post will largely overlap with the original Pwnagotchi installation instructions. If you are familiar with the process of getting your Pwnagotchi up and running, but you are having problems with connecting your iPhone, you can skip directly to the Bluetooth setup section.

So here we go.

#### Build Requirements:

- **Raspberry Pi Zero W** board <span style="font-size: x-small;">- I used the "WH" version - which is exactly the same as the "W" version, only it has the GPIO header pre-soldered.</span>
- **A microSD Card** <span style="font-size: x-small;">- I used a common cheap 16GB microSD that I found laying around my home</span>
- *\[optional\] -* **A case for your Pi Zero** <span style="font-size: x-small;">- I used the vanilla Pi Zero W case (with an opening for the GPIO headers)</span>

#### Step 1: Flashing the Pwnagotchi img to the card

<div>

<span style="font-size: x-small;">The instructions laid out on the <a href="https://pwnagotchi.ai/installation/#flashing-an-image" rel="nofollow" target="_blank">official pwnagotchi website</a> are quite easy to follow. I will, however, add my own version here also, since I had used <a href="https://www.raspberrypi.com/software/" rel="nofollow" target="_blank">Raspberry Pi Imager</a> instead of <a href="https://etcher.balena.io/" rel="nofollow" target="_blank">balenaEtcher</a> for writing the .img to your microSD.</span>

</div>

<div>

- Download the latest Pwnagotchi .img from the <a href="https://github.com/evilsocket/pwnagotchi/releases" rel="nofollow" target="_blank">official releases page</a>. You will actually be downloading a zip file, but some of the tools for flashing might require an .img, so make sure you unzip it after the download.
- Download <a href="https://www.raspberrypi.com/software/" rel="nofollow" target="_blank">Raspberry Pi Imager</a> and install it. Alternatively you can use balenaEtcher or simple dd command.
- Connect your microSD card reader to your computer with the card inside
- Open the Raspberry Pi Imager and select "Use Custom" for the OS, and then select the downloaded .img file. Then select your microSD using Chose Storage in the Imager.
- Click Write and wait until the process is finished.
- Do not remove your card yet, since you will be adding the configuration file to it.

#### Step 2: Configuration file

</div>

<div>

In order to configure your Pwnagotchi, you will be creating a config.toml file in the root of your microSD card. The contents of your configuration file should be based on this example below:

</div>

<div>

\

</div>

> <div>
>
> <div>
>
> </div>
>
> </div>
>
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">main.name = "NAME OF YOUR PWNAGOTCHI"</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">main.lang = "en"</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">main.whitelist = \[</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">  "YOUR HOME NETWORK SSID 1",</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">  "YOUR HOME NETWORK SSID 2"</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">\]</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">\
> > </span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">main.plugins.grid.enabled = false     # see original setup if you want to enable</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">main.plugins.grid.report = false        \# the grid plugin and grid reporting</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">\
> > </span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">ui.web.username = "USERNAME FOR THE WEB UI"   # set this to anything you want</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">ui.web.password = "PASSWORD FOR THE WEB UI"    \#</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">\
> > </span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">\# This is the bluetooth section</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">main.plugins.bt-tether.enabled = true</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">main.plugins.bt-tether.devices.ios-phone.enabled = true</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">main.plugins.bt-tether.devices.ios-phone.search_order = 1</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">main.plugins.bt-tether.devices.ios-phone.mac = "YOUR IPHONE BT MAC ADDRESS"</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">main.plugins.bt-tether.devices.ios-phone.ip = "172.20.10.66"</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">main.plugins.bt-tether.devices.ios-phone.netmask = 24</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">main.plugins.bt-tether.devices.ios-phone.interval = 1</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">main.plugins.bt-tether.devices.ios-phone.scantime = 10</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">main.plugins.bt-tether.devices.ios-phone.max_tries = 0</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">main.plugins.bt-tether.devices.ios-phone.share_internet = true</span>
> >
> > </div>
> >
> > </div>
> >
> > <div>
> >
> > <div>
> >
> > <span style="background-color: white;">main.plugins.bt-tether.devices.ios-phone.priority = 1</span>
> >
> > </div>
> >
> > </div>
>
> <div>
>
> <div>
>
> </div>
>
> </div>

<div>

\

</div>

<div>

\

</div>

<div>

Step 3: First boot of your new pet

</div>

<div>

\

</div>

<div>

Step 4:  Bluetooth pairing

</div>

<div>

\

</div>

<div>

Step 5: Connect to your new pet from your iPhone

</div>

<div>

\

</div>
