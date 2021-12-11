---
title: PVC stuck in terminating
parent: Troubleshooting
grand_parent: Kubernetes
tags: kubernetes, troubleshooting, pvc, stuck, terminating, error

---

# Header
A PVC can be stuck in terminating state
```
kubectl describe pvc -n nextcloud                                                                                                                                       1 ⨯
Name:          pvc-nextcloud-db
Namespace:     nextcloud
StorageClass:  vmware-datastore-01
Status:        Terminating (lasts 6m27s)
Volume:        pvc-eeaf289e-65f0-4bb2-a1d5-2f6829594de9
Labels:        cattle.io/creator=norman
               velero.io/backup-name=nextcloud-20201031000058
               velero.io/restore-name=nextcloud-20201031000058-20201101015253
Annotations:   field.cattle.io/creatorId: user-hgsqt
               pv.kubernetes.io/bind-completed: yes
               pv.kubernetes.io/bound-by-controller: yes
               volume.beta.kubernetes.io/storage-provisioner: kubernetes.io/vsphere-volume
Finalizers:    [kubernetes.io/pvc-protection]
Capacity:      10Gi
Access Modes:  RWO
VolumeMode:    Filesystem
Mounted By:    nextcloud-db-657797459b-glxt8
Events:        <none>
``` 
Look closely at Finalizers: `[kubernetes.io/pvc-protection]`

You can patch this status with `kubectl patch pvc pvc-nextcloud-db -p '{"metadata":{"finalizers": []}}' --type=merge -n nextcloud`