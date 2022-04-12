+++
title = 'ssh options for ancient IOS version'
date = '2020-07-03'
author = 'fitzi'
tags = [ 'IOS', 'linux' ]
draft = false
+++

Connecting to devices running ancient Cisco IOS versions can be a pain as newer ssh clients are not by default supporting weak ciphers and older key exchange algorithms in these old devices only support insecure ciphers.

I had an alias in my profile for this long ago but after upgrading my OS I ran into the issue again, in this case it was the following version of IOS:

```bash {hl_lines=0}
Cisco IOS Software, C3750 Software (C3750-IPSERVICESK9-M), Version 12.2(55)SE6, RELEASE SOFTWARE (fc1)

c3750-ipservicesk9-mz.122-55.SE6.bin
```

I found my previous workaround but this was no longer working, the fix was to also specify the cipher, adding the below as an alias in my .bashrc profile:

```bash {hl_lines=0}
alias ssho='ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 -caes256-cbc'
```