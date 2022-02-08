---
title: Photon OS Rancher host
parent: Setups
grand_parent: Kubernetes
tags: rancher, kubernetes

---

# Photon OS Rancher host


## Change keyboard locale
``` 
tdnf install kbd 
```

Set Swedish keyboard map

```
localectl set-keymap se-lat6
```


## Add user
```
useradd -m -G sudo [USERNAME]
```


## add SSH key
Create directory
```
mkdir ~/.ssh
```

## Static IP
check current interface name with 
```
networkctl
```

```
cat > /etc/systemd/network/10-static-en.network << "EOF"

[Match]
Name=eth0

[Network]
Address=198.51.0.2/24
Gateway=198.51.0.1
DNS=198.51.0.1
EOF
```
Set bit 
```
chmod 644 10-static-en.network
```

Restart services

```
systemctl restart systemd-networkd
systemctl restart systemd-resolved

```