---
title: Edgerouter slow vlan throughput
description: 
published: 1
tags: edgerouter
parent: Stuff
grand_parent: Edgerouter
---

# Slow vlan throughput, both internal and external
Credit to: https://www.ivobeerens.nl/2018/06/04/slow-network-throughput-between-ubiquiti-edgerouter-vlans/

Noticed some issues with slow WAN speeds after moving my clients in to a new VLAN (roughly half duplex).

1. SSH to the edgerouter
2. run 
```shell
show ubnt offload 
```
which should show IPv4 vlan = disabled

```shell
qhrizz@ER-4:~$ show ubnt offload

IP offload module   : loaded
IPv4
  forwarding: enabled
  vlan      : disabled
  pppoe     : disabled
  gre       : disabled
  bonding   : disabled
IPv6
  forwarding: disabled
  vlan      : disabled
  pppoe     : disabled
  bonding   : disabled

IPSec offload module: loaded

Traffic Analysis    :
  export    : enabled
  dpi       : enabled
    version       : 1.564

```

3. Enter configure mode by typing `configure` 
4. Enable VLAN offloading with
```shell
set system offload ipv4 vlan enable 
```
5. `commit`
6. Try out the setting (no rebot required)
7. Save the setting with  `save`
