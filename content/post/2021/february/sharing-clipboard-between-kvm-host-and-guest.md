+++
title = 'sharing clipboard between kvm host and guest'
date = '2021-02-11'
author = 'fitzi'
tags = [ 'kvm', 'linux' ]
draft = false
+++
This took some searching as the actual fix was a little difficult to find as most solutions didn’t include adding the channel to the guest. Thanks to reddit user [0x4c47](https://www.reddit.com/user/0x4c47/) for this fix.

> Open your virt-manager and open the machine. If it’s running, shut it down. Open the hardware details of the virtual machine. Choose “Add Hardware” and choose channel. Then select “com.redhat.spice.0” (or similar) as name and “Spice agent (spicevmc)” as device type. Finish. You’re done. If you didn’t install the SPICE guest tools in the guest yet, do it now. If something still doesn’t work and you’re using Wayland on your host, try Xorg.


Reddit user 0x4c47
https://www.reddit.com/r/linux/comments/asw4wk/to_all_you_virtmanager_and_spice_display_users/