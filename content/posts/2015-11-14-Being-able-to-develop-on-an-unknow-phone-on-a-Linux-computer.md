---
title: "Being able to develop on an unknown phone on a Linux computer"
description: "Everything you have to set up to deploy an Android app to your phone via ADB"
date: 2015-11-14
---


When i tried to deploy a `Hello World` app from my Linux notebook to my `THL 5000` notebook i got this result:
![Alt text](/img/android-dev.png "showing that the phone has only API level 1")

So, my phone has API level 1, nice - NOT!

Searching my ass off, I could nowhere find the device id for this phone... So [Tobi](https://chaos.social/@tbsprs) told me: Brute-Force is the way to go and what should I say: it worked!

So: search for the `51-android.rules` or just save [this one](https://raw.githubusercontent.com/snowdream/51-android/master/51-android.rules)

After that copy it in the right place
```bash
sudo cp /home/username/download/51-android.rules /etc/udev/rules.d/
```
and make it working with
```bash
sudo chmod a+r /etc/udev/rules.d/51-android.rules
```
When plugging now my THL 5000 to my notebook, everything works fine!
