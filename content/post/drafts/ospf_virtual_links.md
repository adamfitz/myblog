+++
title = 'OSPF Virtual Links'
date = '2023-02-??'
author = 'fitzi'
tags = [ 'ospf', 'virtual-links', 'spcor]
draft = true
+++

# OSPF Virtual Links

The overall concept of an OSPF virtual link is simple, a virtual connection from one area 
to join it to the backbone, thorough another area when no physical connection exists.

The configuration could be a little confusing, basically you want 
to specify the "transit" area, which is the area between the backbone area and the partitioned 
area (the area without the physical connection) and also the loopback Ip of teh router in the 
backbone area for example:

< INSERT TOPOLOGY DIAGRAM >

```
# area 1 virtual-link 10.1.1.1
```

Perform this command on the router that connects to the partitioned area ,the transit area and 
also the backbone area.

Generally this should not be considered a design tool, but a tool to repair a network partition