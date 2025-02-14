---
title: "How to play The Curse of Monkey Island on Linux"
description: "This is a step by step example, how to get the third Monkey Island game - The Curse of Monkey Island - running on your Linux system - thank you Ron Gilbert for all this jewels from my childhood "
categories: [linux, gaming, retrogaming, how-to]
tags: [monkey island, retro, 90ties]
date: 2020-04-14
---

As I was always a LucasArts fan in the 90ties, I wanted to use my "free time" (thanks to the Corona lockdown) for playing this old classics.

I bought Monkey Island 1 + 2 on Steam - with the Proton helper, it starts all easy, out of the box.

Monkey Island 3 and 4 I was buying at Gog (it was cheaper) - no help here to get it running for Linux. (it will still run from Steam out of the box, afaik)

For The Curse of Monkey, also known as Monkey Island 3, there are some tricks needed:

I found the page [.playIt](https://www.dotslashplay.it/en/games/monkey-island-3) where it was really easy to get it running.
1. download the 2 scripts `play-monkey-island-3.sh` and `libplayit2.sh`
2. copy the 2 download files from GOG and put it next to your 2 scripts
3. On arch install `icoutils imagemagick innoextract`, see the link above for more infos 
4. `play-monkey-island-3.sh` - and the software should be installed.
5. you can now run Monkey Island 3 with your [ScummVM](https://www.scummvm.org)
