---
title: Photon OS 4 tdnf 404
parent: Troubleshooting
grand_parent: Kubernetes
tags: kubernetes

---

# Photon OS 4 tdnf 404
I started getting a lot of 

```
Error: 404 when downloading https://packages.vmware.com/photon/4.0/photon_updates_4.0_x86_64/x86_64/python3-xml-3.9.1-5.ph4.x86_64.rpm
. Please check repo url.
```

quick fix


1. 
``` 
tdnf clean all 
```
2. 
``` 
tdnf check-update 
```
3. 
``` 
tdnf update 
``` 