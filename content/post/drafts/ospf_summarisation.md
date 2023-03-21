+++
title = 'OSPF Summarisation'
date = '2023-02-??'
author = 'fitzi'
tags = [ 'ospf', 'summarisation']
draft = true
+++

# OSPF Summarisation

- Summarisation can occur on an ABR between one area and the backbone (the router 
with an interface in each area)
- The summary route will only be advertised if you have at least one subnet
falls with the summary range
- A route to Null0 will also be created as a loop prevention mechanism to drop
 packets for destinations where a more specific route does not exist.

Having a subnet within the summary range means that the ABR has an interface with an 
IP address within the summary range.

The summary route will not be created/advertised if the ABR is receiving a subnet within the summary 
range, only a the subnet within the summary range is present on one of the ABRs interfaces.

![OSPF Summarisation](ospf_summarisation.jpg)