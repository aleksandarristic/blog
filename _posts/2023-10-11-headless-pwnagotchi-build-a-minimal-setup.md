---
title: "Headless Pwnagotchi Build - A minimal setup"
date: 2023-10-11 09:36:00 +0000
layout: post
---

In case you are wondering what a Pwnagotchi is, I recommend reading [this excellent article](https://www.evilsocket.net/2019/10/19/Weaponizing-and-Gamifying-AI-for-WiFi-Hacking-Presenting-Pwnagotchi-1-0-0/) by [Evilsocket](https://www.evilsocket.net/).

While there are some very comprehensive DIY tutorials for building a Pwnagotchi, I've noticed a lack of any minimal setup instructions out there. Considering that I had a Raspberry Pi Zero W laying around, I decided to go ahead and make the minimal build. The build is using only the Pi Zero W. There is no screen attached, no battery soldered to the Pi board, no additional antenna.

The goal of the mini-project is to build a Pwnagotchi and connect to it via Bluetooth from an iPhone. The most common problems I've seen out there are exactly Bluetooth related - how to reliably connect to your virtual hacking pet using BT.

Final note before we begin - this blog post will largely overlap with the original Pwnagotchi installation instructions. If you are familiar with the process of getting your Pwnagotchi up and running, but you are having problems with connecting your iPhone, you can skip directly to the Bluetooth setup section.

So here we go.

#### Build Requirements:

- **Raspberry Pi Zero W** board - I used the "WH" version - which is exactly the same as the "W" version, only it has the GPIO header pre-soldered.
- **A microSD Card** - I used a common cheap 16GB microSD that I found laying around my home.
- *[optional]* **A case for your Pi Zero** - I used the vanilla Pi Zero W case (with an opening for the GPIO headers).

#### Step 1: Flashing the Pwnagotchi img to the card

The instructions laid out on the [official pwnagotchi website](https://pwnagotchi.ai/installation/#flashing-an-image) are quite easy to follow. I will, however, add my own version here also, since I had used [Raspberry Pi Imager](https://www.raspberrypi.com/software/) instead of [balenaEtcher](https://etcher.balena.io/) for writing the `.img` to your microSD.

- Download the latest Pwnagotchi `.img` from the [official releases page](https://github.com/evilsocket/pwnagotchi/releases). You will actually be downloading a zip file, but some of the tools for flashing might require an `.img`, so make sure you unzip it after the download.
- Download [Raspberry Pi Imager](https://www.raspberrypi.com/software/) and install it. Alternatively you can use balenaEtcher or simple `dd` command.
- Connect your microSD card reader to your computer with the card inside.
- Open the Raspberry Pi Imager and select "Use Custom" for the OS, and then select the downloaded `.img` file. Then select your microSD using Chose Storage in the Imager.
- Click Write and wait until the process is finished.
- Do not remove your card yet, since you will be adding the configuration file to it.

#### Step 2: Configuration file

In order to configure your Pwnagotchi, you will be creating a `config.toml` file in the root of your microSD card. The contents of your configuration file should be based on this example below:

```toml
main.name = "NAME OF YOUR PWNAGOTCHI"
main.lang = "en"
main.whitelist = [
  "YOUR HOME NETWORK SSID 1",
  "YOUR HOME NETWORK SSID 2"
]

main.plugins.grid.enabled = false     # see original setup if you want to enable
main.plugins.grid.report = false      # the grid plugin and grid reporting

ui.web.username = "USERNAME FOR THE WEB UI"   # set this to anything you want
ui.web.password = "PASSWORD FOR THE WEB UI"

# This is the bluetooth section
main.plugins.bt-tether.enabled = true
main.plugins.bt-tether.devices.ios-phone.enabled = true
main.plugins.bt-tether.devices.ios-phone.search_order = 1
main.plugins.bt-tether.devices.ios-phone.mac = "YOUR IPHONE BT MAC ADDRESS"
main.plugins.bt-tether.devices.ios-phone.ip = "172.20.10.66"
main.plugins.bt-tether.devices.ios-phone.netmask = 24
main.plugins.bt-tether.devices.ios-phone.interval = 1
main.plugins.bt-tether.devices.ios-phone.scantime = 10
main.plugins.bt-tether.devices.ios-phone.max_tries = 0
main.plugins.bt-tether.devices.ios-phone.share_internet = true
main.plugins.bt-tether.devices.ios-phone.priority = 1
```

#### Step 3: First boot of your new pet

#### Step 4: Bluetooth pairing

#### Step 5: Connect to your new pet from your iPhone
