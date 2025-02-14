---
title: "No HDD space left for ubuntu update, make some space at /boot "
description: "A typical Ubuntu problem: you can't make any updates because No HDD space left for ubuntu update, make some space at /boot. Here is a way that you can install again"
categories: [linux, ubuntu, problems, how-to]
tags: [boot, diskspace]
date: 2015-05-21
---

![Alt text](/img/no_hdd_space_left_full.png "your hd is full, make space in /boot")    
your hd is full, make space in /boot
aaaa
make a
```bash
cd /boot ls -la
so you see the `/boot` folder
```
![Alt text](/img/no_hdd_space_left_full_lsla.png "screenshot of the boot folder")   

now delete the stuff you don't need anymore:
```bash
sudo aptitude remove linux-image-3.13.0-49-generic
sudo apt-get remove ...
```
![Alt text](/imgafter_deleting_linux_image.png "ls -la after deleting the linux kernel image")     
(ls -la after deleting the linux kernel image)
