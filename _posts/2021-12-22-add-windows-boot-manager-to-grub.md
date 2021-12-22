---
title: Add Windows Boot Manager to Grub
author: krygennn
date: 2021-11-18 12:00
categories: [Blogging, Operating-Systems]
tags: [linux, windows, grub, boot-managers]

---

If you want to add windows boot manager to grub, follow these steps;

1-Download **os-prober**<br>
2-Run `sudo os-prober`<br>
If you see something like that, you are good to go.
```bash
-> sudo os-prober
/dev/nvme1n1p1@/EFI/Microsoft/Boot/bootmgfw.efi:Windows Boot Manager:Windows:efi
```
<br>
3- Add this line, **GRUB_DISABLE_OS_PROBER=false**, at the end of the **/etc/default/grub** file.<br>
4- Run `sudo grub-mkconfig -o /boot/grub/grub.cfg`<br>
<br>
Done !
