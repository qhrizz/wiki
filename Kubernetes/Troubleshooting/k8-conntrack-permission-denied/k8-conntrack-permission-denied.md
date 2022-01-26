---
title: Rancher Conntrack permission denied error
parent: Troubleshooting
grand_parent: Kubernetes
tags: kubernetes
---

# Conntrack permission denied error
Sometimes Rancher crashed on start with the error message 

```
I0729 11:13:35.953517      27 conntrack.go:103] Set sysctl 'net/netfilter/nf_conntrack_max' to 131072
F0729 11:13:35.953542      27 server.go:495] open /proc/sys/net/netfilter/nf_conntrack_max: permission denied
```

To temporarily fix until next reboot, try 
```
sysctl -w net/netfilter/nf_conntrack_max=131072
```

# In Ubuntu (atleast)
1. Edit /etc/modules 
2. At the end of the file, add nf_conntrack and save
3. Edit /etc/sysctl.conf and add "net.netfilter.nf_conntrack_max=131072" and save
4. Reboot 