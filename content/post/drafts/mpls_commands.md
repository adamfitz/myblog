+++
title = 'MPLS Commands'
date = '2023-05-??'
author = 'fitzi'
tags = [ 'mpls', 'ldp']
draft = true
+++

## MPLS Show Commands

LDP session and protocol information

```show mpls ldp parameters```

LDP neighbour information (peer address, interface, detailed session info)

```
show mpls ldp neighbor
show mpls ldp neighbor detail
```

MPLS interface information

```
show mpls interfaces
show mpls interfaces detail
```

MPLS interfaces and their configured neighbours

```
show mpls ldp discovery vrf $vrf_name
show mpls ldp discovery all
```

## Table Information

LIB - **L**abel **I**nformation **B**ase

```
show mpls ldp bindings
```

LFIB - **L**abel **F**orwarding **I**nformation **B**ase

```
show mpls forwarding
```

FIB - **F**orwarding **I**nformation **B**ase

```
show cef
show crf $prefix
```





