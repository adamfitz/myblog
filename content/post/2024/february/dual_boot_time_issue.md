+++
title = 'Dual boot Windows/Linux time sync issues'
date = '2024-02-25'
author = 'fitzi'
tags = [ 'windows', 'dual boot']
draft = false
+++

# Windows time issues when dual booting

There is an annoying issue when dual booting Windows and Linux where after using linux and the restarting into windows the time is wrong.   According to tthe information [here](https://help.ubuntu.com/community/UbuntuTime#Multiple_Boot_Systems_Time_Conflicts) this is because most operating systems store the time on the hardware clock as UTC by default, but Windows does not and stores it instead in local time.

There is a registry change that will will fix this issue as described [here](https://www.elevenforum.com/t/how-to-make-win-11-sync-time-on-boot.12189/), by changing (or adding if it does not exist) the below registry key:

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation] → "RealTimeIsUniversal" = qword:00000001
```