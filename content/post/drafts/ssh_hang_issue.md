+++
title = 'ssh hang problems Cisco 3750g'
date = '2023-0?-??'
author = 'fitzi'
tags = [ 'cisco', 'ssh hang']
draft = true
+++



- problems with ssh connectivity to an old switch
- ssh would connect then after a random time maybe 10-30 seconds the terminal would just hang (no more input but no error)
- nothing in the logs
- no deviation in cpu/memory
- did not happen to an adjacent identical switch (same mode and software version) 
- reviewing the config I noticed two SVIs, one in the data vlan and one in the wireless vlan
- removing the wireless vlan SVI fixed the issue (it was there presumably for testing in the past)
- what was the issue?