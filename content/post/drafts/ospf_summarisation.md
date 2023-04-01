+++
title = 'OSPF Summarisation'
date = '2023-03-??'
author = 'fitzi'
tags = [ 'ospf', 'summarisation']
draft = true
+++

# OSPF Summarisation

## Internal summarisation (for routes learned/injected WITHIN the OSPF AS)

- Summarisation can only occur on an ABR between one area and the backbone (the router with an interface in each area)
- The summary route will only be advertised if you have at least one subnet falls with the summary range
- A route to Null0 will also be created as a loop prevention mechanism to drop packets for destinations where a more 
specific route does not exist.

Having a subnet within the summary range means that the ABR has an interface with an IP address within the summary 
range.

The summary route will not be created/advertised if the ABR is receiving a subnet within the summary 
range, summary route is only created if the subnet within the summary range is present on one of the ABRs interfaces.

OR 

If the ABR is injecting/advertising static routes that fall within teh summary range.  <-- NO

Will not originate a summary route from a static route configured on the device.

![OSPF Summarisation](ospf_summarisation.jpg)


## External summarisation (for routes learned from OUTSIDE the OSPF AS)


