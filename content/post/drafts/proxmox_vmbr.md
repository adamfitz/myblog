+++
title = 'Proxmox vnet for "internal" Network'
date = '2024-04-07'
author = 'fitzi'
tags = [ 'proxmox', 'vmbr']
draft = true
+++

# Create a new vnet  in proxmox

- In the web gui
- Select pve
- Select create
- Select Linux bridge
- Set the name to vmbr**X** where x is a number
- Select one of the physical NICs on the host, in my case I used a port that was down/unplugged just to setup the bridge
 (all traffic will be internal to the host anyway)
- Type the name of the physical NIC in the "bridge ports" dialog box (get the interface name from pve --> Network in the
 GUI or from the proxmox cli with "ip -br addr")
- Add a comment so in 12 months time you have some idea why the vnet was created and perhaps what it is used for
- Select create

**NOTE:** Remember to press the "Apply Configuration" button to create the new bridge, as proxmox will allow you to
 attach the vnet to a VM before applying the config but if you then try and start the VM you will get an error that the
  bridge / vmbrX does not exist.



