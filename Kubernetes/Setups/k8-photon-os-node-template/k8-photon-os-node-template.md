---
title: Photon OS Node Template
parent: Setups
grand_parent: Kubernetes
tags: kubernetes, photon, template

---

# Node Template [WORK IN PROGRESS]
1. Download the minimal iso from https://github.com/vmware/photon/wiki/Downloading-Photon-OS
2. Set Disk size,CPU, Memory etc and boot fromt the ISO

3. Install sudo 
```
tdnf install sudo
```
4. Remove instances folder
```
sudo rm -rf /var/lib/cloud/instances
```
5. To prevent duplicate DHCP addresses, run: 
```
truncate -s 0 /etc/machine-id
sudo rm /var/lib/dbus/machine-id
```

6. Install open-iscsi and update/upgrade
```
sudo tdnf install open-iscsi -y
tdnf update && tdnf upgrade 
```

7. Shut down the machine
8. Clone to Template
9. In Rancher, add new node template and select the new template
10. Rancher cloud-init should take care of the rest.