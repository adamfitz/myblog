+++
title = 'Site-to-Site IPSEC VPN configuration'
date = '2011-12-29'
author = 'fitzi'
description = "s2s vpn"
tags = [ 'vpn', 'cisco' ]
draft = false
+++

**This is a repost of a site to site VPN config with low end Cisco ISR routers, I wrote a few years ago as a proof of concept for replacing a small MPLS network for a managed service provider I was working for. At the time I had not setup ipsec VPNs before so I documented the steps I went through here..**.

The purpose of this post is to document the configuration commands and give an outline to the process of configuring site to site VPN connection as I had a little trouble getting a clear picture of what each command did and why it was being used. In this instance the two end points are Cisco 857w routers connected directly to the internet with PAT enabled.

I started with a general config and a link to this site to site vpn post from [Learn Networking with Me](http://learnnetworkingwithme.wordpress.com/2011/12/18/cisco-ipsec-site-to-site-configuration/ "Learn Networking with Me") which gives some good explanations of what to do and why. The VPN configuration on each of my routers is as follows:

**Router 1 – Home**

```bash {hl_lines=0}
crypto isakmp policy 1
 encr aes
 authentication pre-share
 group 2
crypto isakmp key Pa55w0rd address $OfficeRouterPublicIPAddress no-xauth
!
!  
crypto ipsec transform-set Tunnel_to_Office esp-aes esp-sha-hmac
!
crypto map Home_To_Office 10 ipsec-isakmp
 set peer $OfficeRouterPublicIPAddress
 set transform-set Tunnel_to_Office
 match address 101
 ```

**Router 2 – Office**

```bash {hl_lines=0}
crypto isakmp policy 1  
 encr aes  
 authentication pre-share  
 group 2  
crypto isakmp key Pa55w0rd address $HomeRoutersPublicIPAddress no-xauth  
!  
!  
crypto ipsec transform-set Tunnel_to_Home esp-aes esp-sha-hmac   
!  
crypto map Office_To_Home 10 ipsec-isakmp   
 set peer  $HomeRoutersPublicIPAddress
 set transform-set Tunnel_to_Home   
 match address 101
 ```


Referring to Learn Networking with Me’s link the **crypto isakmp policy** sets up phase 1 tunnel with the peer router. The **crypto ipsec transform** configures the ipsec tunnel encryption scheme and calls it Tunnel\_to\_Office. The **crypto map** is the policy which defines the data flows and the crypto peer to send it to, here the office and home routers public IP is defined as each peer and refers to the transform set for the type of encryption to use.

The **match 101** refers to the access list which will allow or deny traffic through the tunnel. I stumbled a little bit with these as I have had little practice with access lists in the cisco world.

**Router 1 – Home**

```bash {hl_lines=0}
access-list 101 permit ip 10.23.0.0 0.0.0.255 10.0.0.0 0.255.255.255
```

Here this access list is permitting the IP range 10.23.0.0/24 to send traffic to any ip address in the 10.0.0.0/8 range. The local LAN at home is 10.23.0.0/24 and if packets are sent to any address in the 10.0.0.0/8 range it will be sent across the VPN tunnel.

**Router 2 – Office**

```bash {hl_lines=0}
access-list 101 permit ip 10.0.0.0 0.255.255.255 10.23.0.0 0.0.0.255
```

The access list on the office router is the reverse of the home router allowing all traffic from the 10.0.0.0/8 being sent to 10.23.0.0/24 to be sent across the tunnel.

Once the tunnel and the access list for the tunnels are setup I applied the crypto map to an interface on each router so that when traffic was sent to each specified network range it could be matched and forwarded across the VPN tunnel. The crypto map needs to be applied to the Public interface (in this instance) so that when traffic is sent to an address which is not on the local LAN the packet will use the default route to the Dialer1 interface which is then matched by the **match address 101** command in the crypto map and sent down the VPN tunnel.

For the office router I needed to put a static route in for my home network as it already had routes for the 10.0.0.0/8 ranges being forwarded to another MPLS router in the office. Otherwise the traffic for 10.23.0.0/24 would have been forwarded incorrectly to the local MPLS router rather than to the VPN tunnel.

**Router 2 – Office**

```bash {hl_lines=0}
ip route 10.0.0.0 255.0.0.0 10.2.0.254 
ip route 10.23.0.0 255.255.255.0 Dialer1
```

At this stage I knew I was pretty close with this configuration, to bring up the VPN tunnel however I needed to ping from the local LAN interface on my home router to a subnet in the office, this would match the traffic, bring up the VPN and successfully pass the traffic across the tunnel. But I just couldn’t get the tunnel to come up, I would traceroute to a subnet in the office but the packet was hitting the dialer interface and trying to go to the internet without being matched and I had no idea why, so I opened a TAC case.

The engineer was awesome and had a very good understanding of what I was trying to achieve, a couple of emails later he suggested that Phase 2 was up but the traffic was not being sent across the VPN because of NAT overload, all the outbound traffic was being NAT’ed.

**Router 1 – Home**

```bash {hl_lines=0}
ip nat inside source list 199 interface Dialer1 overload  
!  
!  
access-list 199 deny ip 10.23.0.0 0.0.0.255 10.0.0.0 0.255.255.255  
access-list 199 permit ip 10.23.0.0 0.0.0.255 any
```

Here we have an extended access list assigned to the Dialer interface allowing NAT overloading (line 1) and below are the IP addresses that are denied or allowed. The first line in the access list indicates that packets sent from the 10.23.0.0/24 network with a destination of 10.0.0.0/8 will be denied the application of NAT however the next line indicates that traffic sent from the 10.23.0.0/24 network to **any other** address ranges would be allowed. The reason for this is to still allow access to the VPN as well as having the ability to port forward in and out of the local LAN.

**Router 2 – Office**

```bash {hl_lines=0}
ip nat inside source list 199 interface Dialer1 overload  
!  
!  
access-list 199 deny ip 10.0.0.0 0.255.255.255 10.23.0.0 0.0.0.255  
access-list 199 permit ip 10.2.0.0 0.0.0.255 any
```

Here on the office router the access list in reverse to the home router, permitting the local LAN addresses to NAT but denying NAT from 10.0.0.0/8 addresses to 10.23.0.0/24.

I had the following debugs running on my router, **terminal monitor**, **debug crypto isakmp**, **debug crypto ipsec**, after running a **clear crypto session** and running another ping from the LAN to an address at the office it was about this point that the screen exploded with output and I knew that I had finally got it up and running.

This configuration is not a particularly difficult one, however as I said earlier I had not setup a site to site VPN before and it took me a good few days of perseverance to get to this point. I am thankful for the help from the TAC guys as well as the information gleaned from the Site to Site VPN article at [Learn Networking with Me](http://www.learnnetworkingwithme.wordpress.com "Learn Networking with Me").

The best thing about this little project was that as well as achieving something practical I got to learn quite a few things along the way.