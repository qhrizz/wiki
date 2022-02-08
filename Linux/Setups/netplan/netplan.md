--- 
title: netplan template
parent: Setups
grand_parent: Linux
tags: linux, netplan
---

# netplan.yaml template
```
network:
    ethernets:
        ens160:
            addresses:
            - 10.10.10.10/24
            gateway4: 10.10.10.1
            nameservers:
                addresses:
                - 1.1.1.1
                - 8.8.8.8
                search: []
            optional: true
    version: 2
```
