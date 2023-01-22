+++
title = 'New desktop build no POST and random crashes'
date = '2023-01-22'
author = 'fitzi'
tags = [ 'new pc', 'no POST', 'EXPO']
draft = false
+++

# New PC build that would not post

Over the xmas break I helped a mate put together a new desktop, consisting of an ASUS PRIME X670-P board, 32gb ram, 
Ryzen 7 7700 and an MSI Ventus 3x RTX 4080 with an 850w Seasonic PSU in a Fractal case.

We assembled the hardware and proceeded to spend a couple of hours trying to work out why it would not POST.  No beep 
codes, no output on the screen.  Eventually we started to strip the machine back to nothing but the RAM, when we 
noticed that removing (unplugging) one of the fans from the Nocturna cooler fans the system proceeded to POST.

Long story short I initially connected both the Nocturna cooler fans to a 'Y' splitter cable and attached this to a 
motherboard fan header.  This was stopping the box from posting, I assume, this was due to the fact (according to the 
MB manual) that the header would only provide 1.5v.  Realising the mistake and connecting each fan to their own 
respective header 'solved' the issue.  I suspect that if I had accidentally connected the 'Y' splitter cable to the AIO
header (right next to the CPU fan header =/) instead of the fan header all would have been well, as the MB manual 
indicates the AIO header provides 3.5v and would likely have been enough to run both fans off the one header. 

As there is no PC speaker in the case or on the board anymore meant a lot of confusion when no beep codes were
 generated even with all the RAM removed from the system, showing my age here as most/all the desktops I have built 
'recently' contained a speaker of some sort.

The second and more frustrating issue we ran into happened after the system was up and running.  Windows installed, all 
drivers updated, Steam and some games installed etc the machine would randomly crash (kernel panic), when running *ANY* 
game regardless of the title eg: Valheim, Fortnight or Elden ring to name a few.  

After a *LOT* of testing, troubleshooting and reinstalling, my mate eventually found that it was a RAM issue which was 
fixed by enabling the EXPO setting in the BIOS.

This is a bit confusing as from what I found when attempting to research the fix on the internet is that most people 
reported this issue when this setting is enabled (not disabled).  This is (it seems) due to the fact that the EXPO 
feature is an automatic overclocking mechanism for specifically supported RAM modules.

I had not seen this issue before as I am using the ASUS B550E ASUS board with DDR4 with no corresponding setting 
similar to EXPO as this is only for setting overclocking profiles specifically for DDR5 memory, 
[more details about EXPO can be found here](https://www.digitaltrends.com/computing/what-is-amd-expo/).

For a simple PC build this took a surprising amount of time and frustration trying to get to the bottom of the RAM 
issue as it was just a hard crash with no info in the event logs etc to point us in the right direction.