---
title: vSphere storage provider "file not found" when forcibly removed worker node
parent: Troubleshooting
grand_parent: Kubernetes
tags: kubernetes, storage, vsphere

---

# vSphere storage provider "file not found" when forcibly removed worker node
If you accidently deleted a worker node without draining it, vmware will forcibly remove the vmdk files. This causes issues with Rancher since it thinks there is a vmdk attached which it cannot find now.

![vsphere-error-file-not-found1.png](vsphere-error-file-not-found1.png)

1. Log in to rancher and go to your cluster -> Storage classes
2. Choose your storage
3. Locate the failed disks 
![vsphere-error-file-not-found2.png](vsphere-error-file-not-found2.png)
4. Delete the disks that are failing, once that is done the error messages in vSphere should disappear