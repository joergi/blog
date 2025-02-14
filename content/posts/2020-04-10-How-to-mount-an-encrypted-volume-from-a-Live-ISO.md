---
title: "How to mount an encrypted volume from a live ISO"
description: "if you desstroyed the boot section on your Linux computer and you want to access your data, you will need to encrypt the volume from a live ISO"
date: 2020-04-10
---

If you destroyed somehow your Linux system, which has an encrypted volume, and you want to log into this, but booting is not working, boot from a live ISO, and type: 
```shell
lsblk -f
``` 
you will see `sda1` oder `sdb1` as your encrypted system
![Alt text](/img/encrypted.png "encrypted")

this decrypt it and mount it (for example sda1)
```shell
sudo cryptsetup luksOpen /dev/sda1 crypted_sda1
Enter passphrase for /dev/sda1:
``` 

after entering the passphrase you can see that this is now decrypted
![Alt text](/img/decrypted.png "decrypted")

you are now able to mount the decrypted volume:
```shell
sudo mount /dev/mapper/crypted_sda1 /mnt
```
![Alt text](/img/mounted.png "mounted")
As you see here, the volume is now mounted to `/mnt`

You can then log into your encrypted volume by manjaro-chroot (example for Manjaro system)
```shell
sudo manjaro-chroot /mnt
```
(i read that non Manjaro users use `mhwd-chroot` but I haven't tried it)

As you can see, you are now in your own filesystem.
![Alt text](/img/chroot.png "chroot")
You are now able to install new packages or delete old ones with `apt install xxx` or `pacman -S xxx`
