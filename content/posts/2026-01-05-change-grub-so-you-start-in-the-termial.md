---
title: "Change GRUB so you start in the terminal"
description: "when your computer is not starting anymore, you need to start in the terminal mode"
date: 2026-01-05
---

# Problem: Copmuter is not starting at all anymore
I'm using on one of the computer Manjaro Linux.  
Unfortunately it seems I deleted something really important on the last upgrade.  
The computer starts, I can decrypt my SSD, and I see the login screen. So far so good.  
But if I log in, I then see only a black screen with the mouse. But I can't move the mouse and also no keyboard is working.    
Also the shortcut for going into termimal mode (CTRL + ALT F3 / F4...) is not reacting.

# Solution: Change the Grub
Go into the GRUB menu, and change the code in there:
Add:
```
$ systemd.unit=multi-user.target
```

it should look like, this, add it at the end of the linux line:
```
linux /boot/vmlinuz-........ systemd.unit=multi-user.target
```

Save and reboot by pressing: `CTRL x` 

# Go back to Gnome GUI after fixing 
```
sudo systemctl enable gdm
```