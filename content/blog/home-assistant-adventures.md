+++
title = "Home Assistant Adventures"
date = 2026-05-16
[taxonomies]
  tags = ["life", "fun", "home", "hobbies"]
[extra]
  toc = false
+++

Back having soem fun with home automation thanks to an old NAS and Home Assistant. Its been a few years since I've used it last, when I ran it on a Raspberry Pi 3 and it's come a long way since!
<!-- more -->
I've been super impressed wwith how much easier it is to set up; without doing too much research I ended up with the following:

* Intel NUX w/ 16GB ram
* Sonoff Zigbee 3.0 USB Dongle Plus
* Snoff Temp/Humidity sensors
* Generic chinese aircon wifi module

Setting up HA was as simple as following the instructions on the website. What was not simple was accessing it! Turns out that there are now quite aggressive policies for detecting local network devices built into browsers and OS's that stopped the web page dash from loading. Using the app connected instantly, so I knew it was working and that it was simply a local issue. Some Googling eventually got me to the answer.

Half a day later I had everything working. ZigBee really is as close to plug and play as I could have imagined, a few years ago this all took a lot more effort and I recall having to create my own custom dash in YAML just to set it all up how I wanted.

I can finally set a button again to turn off the house... what a time to be alive 😆

Carlo

