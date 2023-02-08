+++
title = 'Anycast HSRP'
date = '2023-02-??'
author = 'fitzi'
tags = [ 'anycast hsrp', 'fabricpath']
draft = true
+++

# Anycast HSRP Basics

Anycast HSRP is a fabricpath feature that allows the use of up to 4 active gateways by creating
an anycast bundle which is an association between a set of vlans and the anycast switch ID.

The difference between Anycast HSRP and VPC+ is that vpc+ only allows for two active gateways.

Unlike in regular HSRP where only one gateway is responsible for forwarding traffic in anycast 
HSRP, all gateways are actively forwarding traffic.  There are three states 
that an anycast HSRP gateway can be in: Active, Standby or Listen state.

Even though an Active and standby gateway is defined, the Active gateway is not the only device 
forwarding traffic.  In Anycast HSRP, all gateways regardless of their state are responsible
 for forwarding traffic.

The purpose of the Active gateway is to advertise the Anycast switch ID (ASID) to the leaf 
switches.  The Anycast switch ID is the same as the [emulated switch ID]('https://www.cisco.com/c/en/us/td/docs/switches/datacenter/sw/6_x/nx-os/fabricpath/configuration/guide/b-Cisco-Nexus-7000-Series-NX-OS-FP-Configuration-Guide-6x/b-Cisco-Nexus-7000-Series-NX-OS-FP-Configuration-Guide-6x_chapter_0100.html#concept_910E7F7E592D487F84C8EE81BC6FC14F) 
except the ASID is shared across multiple gateways. 

In turn the leaf switches learn the ASID via fabricpath and use this to reach the HSRP 
gateway for a specific vlan, the difference in this case is that the ASID is shared 
across multiple gateways (up to max of four) and all gateways are actively forwarding traffic.
