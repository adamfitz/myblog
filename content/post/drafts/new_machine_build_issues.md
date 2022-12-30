+++
title = 'New build and no POST'
date = '2022-12-30'
author = 'fitzi'
tags = [ 'new pc', 'no POST']
draft = true
+++

# New PC build that would not post

Put a new desktop together with/for a mate, was an ASUS (P6670??) board, 32gb ram, Ryzen 7 7700 and an MSI Ventus 3x RTX 
4080 with an 850w Seasonic PSU in a Fractal case.

After assembling the hardware we proceeded to spend a couple of hours trying to work out why it would not POST.  No beep 
codes, no output on the screen.  Eventually we started to strip the machine back to nothing but the RAM, when we 
noticed that when one of the Nocturna cooler fans was un plugged we got the box to POST.

Long story short while unplugging and removing hardware we pulled out one of the Nocturna cooler fans which had been 
plugged into a 'Y' splitter cable that was attached to one of the fan headers on the motherboard.  This was stopping the box 
from posting, I assume (according to the MB manual), this was due to the fact that the MB fan header would only provide 
1.5v, connecting the fans each to separate MB fan headers 'solved' the issue.  I suspect that if we had accidentally 
connected the 'Y' splitter cable to the AIO header (right next to the CPU fan header =/) instead of the fan header all 
would have been well, as the MB manual states the AIO header will provide 3.5v and would probably have been enough to 
run both fans off the one header. 

As there is no PC speaker in the case or on the board anymore this meant a lot of 'feeling' our way in the dark due to the 
fact that no beep codes were being generated, even when I pulled all the RAM out of the system.

The second and more frustrating issues was we got over the above hurdle and the machine was up and running, Windows and 
steam installed, latest Nvidia drivers, the PC would randomly crash when running *ANY* game, regardless of the title.

After lots of testing, troubleshooting and reinstalling, my mate eventually found that the EXPO setting in the BIOS was enabled(?), 
which was causing incorrect/wrong timings to be applied to the RAM modules which caused the random crashes that also 
wrote nothing to the event logs or game logs, disabling(?) the EXPO setting in the BIOS fixed the stability issue. 



