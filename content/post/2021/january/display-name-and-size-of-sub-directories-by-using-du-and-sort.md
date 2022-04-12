+++
title = 'Display name and size of sub directories by using du and sort'
date = '2021-01-24'
author = 'fitzi'
tags = [ 'linux' ]
draft = false
+++
I used the below command to go through the contents of my google drive (free plan) to find out what I can remove to remain under the 15gb limit, which also include gmail size. I would have found this on the internet somewhere like stack exchange, but I dont have the original link.

```bash {hl_lines=0}
du -hs * | sort -h
```