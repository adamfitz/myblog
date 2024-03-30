+++
title = 'Troubleshooting OSPF adjacency with Wireshark'
date = '2023-03-19'
author = 'fitzi'
tags = [ 'ospf basics', 'ospf adjacency', 'IOSXE', 'spcor']
draft = false
+++

In this post I will demonstrate how to use wireshark to troubleshoot an area type mismatch.  I came across this while 
labbing basic OSPF configuration and finding that one of the neighbour adjacencies would not form.  The issue was that 
on router NSSA-R1 I had correctly configured ```area 23 nssa``` to enable the NSSA area, however on the adjacent router
(IOS-XE-2) I had accidentally configured ```area 23 stub```.

The relevant lab topology I am using is as follows:

![basic lab topology](/images/tshoot_ospf_adj_topology_1.png)

When I found that the adjacency was not forming I started by checking the basics:
- network/subnet mask matched
- I could ping the adjacent interface
- I didn't change the MTU or any other parameters (hello/dead etc)
- re-checked the config on both routers (totally missing the incorrect config statement)

From here I wanted to get a wireshark capture of the hello traffic to see what I missed.  I am labbing with eve-ng so I 
used the ssh remote capture feature of wireshark (SSH remote capture:sshdump), I had not used this before so I followed 
this [blog post](https://netdevops.me/2020/using-wireshark-remote-capture-with-eve-ng/) from Roman Dodin and found 
that it is very easy to set it up.  In the capture file from both router interfaces I could see hello packets being 
sent and received as expected.

Digging into the hello packet from NSSA-R1 (192.23.0.2) I could see that the NSSA flag is set in the OSPF options:

![basic lab topology](/images/tshoot_ospf_adj_nssa_hello_1.png)

Checking the corresponding hello packet in the capture file for the IOS-XE-2 router, I noticed that NSSA flag was 
missing and hence being reported in the capture as not supported:

![basic lab topology](/images/tshoot_ospf_adj_ios-xe-2_hello_1.png)

At this point I knew there was a configuration problem and backtracked again to verify the configuration on both devices
 here I found that I had misconfigured the area as a stub area on the IOS-XE-2 router.

![IOS-XE-2 incorrect config](/images/tshoot_ospf_adj_ios-xe-2_stub.png)

As expected after removing the stub config from the IOS-XE-2 router and correctly setting the nssa 
area type the neighbour adjacency formed:

![IOS-XE-2 correct config](/images/tshoot_ospf_adj_ios-xe-2_nssa.png)

![IOS-XE-2 correct config](/images/tshoot_ospf_adj_ios-xe-2_nssa_2.png)

![basic lab topology](/images/tshoot_ospf_adj_ios-xe-2_neigh_1.png)

![basic lab topology](/images/tshoot_ospf_adj_nssa_neigh_1.png)
