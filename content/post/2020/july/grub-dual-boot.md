+++
title = 'grub and dual booting linux/window'
date = '2020-07-02'
author = 'fitzi'
tags = [ 'grub', 'linux', 'dual boot' ]
draft = false
+++

If you are dual booting linux and windows somewhere along the way you might find that the grub boot loader gets overwritten and you can only boot one operating system.

This is usually an easy fix, set your bios to boot from you linux hdd and once linux has loaded open a terminal and type the following:

```bash {hl_lines=0}
sudo update-grub
```

This will cause grub to regenerate its config file which should find you windows (or other OS installs), add them all to the boot loader menu and reinstate grub to the MBR.