+++
title = 'writing ISO to USB in linux'
date = '2020-07-04'
author = 'fitzi'
tags = [ 'linux' ]
draft = false
+++

Finally writing this down here because I always forget this command and have to either look it up again or go back through my command history.

The correct syntax for using the dd application to write an ISO image to a removable drive is as follows:

```bash {hl_lines=0}
sudo dd if=$isoImageName of=/dev/sda
```

The **incorrect** syntax is to specify the partition, because the operation is writing an ISO image which consists of a single track:

```bash {hl_lines=0}
sudo dd if=$isoImageName if=/dev/sda1
```