+++
title = 'Ubuntu 22.04 wireless adapter issue dual boot with Windows 10'
date = '2022-12-30'
author = 'fitzi'
tags = [ 'ubuntu', 'wireless', 'asus-B550-E']
draft = false
+++


I dual boot Ubuntu 22.04 and Windows 10 on my desktop machine which is using the onboard wireless provided by 
my ASUS B550-E motherboard.  I noticed that sometimes when I booted into Ubuntu the wireless adapter was reported missing 
by the operating system. 

I found that this only happens if I reboot into Ubuntu after first using Windows 10.  Digging deeper I 
found an [explanation of the issue on the kernel.org wiki](https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi#about_dual-boot_with_windows_and_fast-boot_enabled) 

The issue is due to Windows not completely shutting down the machines hardware  on a reboot, this is to try and make the 
subsequent startup time quicker.  The fix (at least in my case) is to disable fast startup 
([detailed steps here](https://www.asus.com/support/FAQ/1045548/)) in Windows power options, it seems that 
Windows will then properly 'shut down' the hardware and allows the wireless module to be loaded correctly 
when ubuntu starts up.