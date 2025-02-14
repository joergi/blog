---
title: "Using XIOAMI gamepad with Linux"
description: "What you have to do on Linux to get the Xiaomi gamepad running - happy gaming"
date: 2016-01-14
---

![Alt text](https://web.archive.org/web/20221014114121if_/https://xiaomi-mi.com/uploads/CatalogueImage/2_13621_1429630637.jpg "XIAOMI Gamepad 2")

I have the XIAOMI gamepad, a cheap gamepad which looks like the classical XBOX gamepad.
When I used it first with my Fire TV Stick, it worked like a charm. I wanted to try it with my Linux and with Steam.

When i used it in a game it was totally uncalibrated. Pressing the joystick to the top position had no effect at all on my game character.
![Alt text](/img/xiaomi-jscalib.png "screenshot form jstest-gtk")
So, i had to calibrate with a small programm, which I install on the shell with:
```bash
sudo aptitude install jstest-gtk
```
start it with:
```bash
jstest-gtk
```

You should see the following pictures...
1. Press the `calibration` button
2. Press the `start calibration` button
3. Rotate the joystick (all joysticks on your gamepad in a circle
4. Move the joystick to `up` - `middle` - `down` - `middle` - `left` - `middle` - `right` - `middle`
5. Press the OK button
6. Save the profile

After restarting Steam, it worked perfectly in the game.
