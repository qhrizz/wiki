---
title: Ubuntu 18.04 simple node template
parent: Setups
grand_parent: Kubernetes
tags: kubernetes, node, template, vsphere

---

# Node Template
1. Install a clean Ubuntu 18.04
2. Remove 
```
sudo rm -rf /var/lib/cloud/instances
```
3. To prevent duplicate DHCP addresses, run: 
```
truncate -s 0 /etc/machine-id
```
4. 
```
sudo rm /var/lib/dbus/machine-id
```
5. 
```
Install cloud-init: sudo apt install cloud-init -y
```
6. 
```
Install open-iscsi: sudo apt install open-iscsi -y
```
7. Shut down the machine
8. Clone to Template
9. In Rancher, add new node template and select the new template
10. Rancher cloud-init should take care of the rest.