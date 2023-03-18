+++
title = 'OSPF basics - Passive Interface'
date = '2023-03-11'
author = 'fitzi'
tags = [ 'ospf basics', 'passive interface', 'IOSXE', 'IOSXR', 'spcor']
draft = false
+++

# OSPF Basics - passive interface

Using the ```passive interface``` or ```passive enable``` command in OSPF stops an adjacency forming over a specific 
router interface.  An interface that has been set to passive still has its IP address injected into the OSPF domain 
and is seen as a STUB network.

This allows interface addresses to be advertised into the OSPF domain but also stops LSA generation / flooding from 
occurring for interfaces where an adjacency is not expected to be formed. 

Differences between IOS XE and IOS XR configuration commands:

**IOS XE**

```
router ospf 23
    passive interface default       # enable globally
    passive interface g1            # enable for specific interface
    no passive interface g1         # disable for specific interface
```

**IOS XR**

```
router ospf 23
    passive enable                  # enable globally (all areas / interfaces
    area 11
        passive enable              # enable at area level (interfaces within the area)
        interface <int-id>
            passive enable          # enable for specific interface
            passive disable         # disable for specific interface
```

