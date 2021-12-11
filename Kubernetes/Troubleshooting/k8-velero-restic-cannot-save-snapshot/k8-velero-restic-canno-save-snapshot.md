---
title: Restic fails backing up PVs 
parent: Troubleshooting
grand_parent: Kubernetes
tags: kubernetes

---


# Restic fails backing up PVs 
```
velero backup logs cl1-wikijs-manuell | grep error
time="2021-09-15T17:27:06Z" level=info msg="1 errors encountered backup up item" backup=velero/cl1-wikijs-manuell logSource="pkg/backup/backup.go:427" name=wikijs-8d87f4fd5-xkqrg
time="2021-09-15T17:27:06Z" level=error msg="Error backing up item" backup=velero/cl1-wikijs-manuell error="pod volume backup failed: error running restic backup, stderr=Save(<data/aed059f00a>) returned error, retrying after 507.606314ms: wrote 0 bytes instead of the expected 1184057 bytes\nSave(<data/aed059f00a>) returned error, retrying after 985.229971ms: wrote 0 bytes instead of the expected 1184057 bytes\nSave(<data/aed059f00a>) returned error, retrying after 803.546856ms: wrote 0 bytes instead of the expected 1184057 bytes\nSave(<data/aed059f00a>) returned error, retrying after 1.486109007s: wrote 0 bytes instead of the expected 1184057 bytes\nSave(<data/aed059f00a>) returned error, retrying after 2.070709754s: wrote 0 bytes instead of the expected 1184057 bytes\nSave(<data/aed059f00a>) returned error, retrying after 3.67875363s: wrote 0 bytes instead of the expected 1184057 bytes\nSave(<data/aed059f00a>) returned error, retrying after 4.459624189s: wrote 0 bytes instead of the expected 1184057 bytes\nSave(<data/aed059f00a>) returned error, retrying after 6.775444383s: wrote 0 bytes instead of the expected 1184057 bytes\nSave(<data/aed059f00a>) returned error, retrying after 15.10932531s: wrote 0 bytes instead of the expected 1184057 bytes\nSave(<data/aed059f00a>) returned error, retrying after 13.811796615s: wrote 0 bytes instead of the expected 1184057 bytes\nFatal: unable to save snapshot: wrote 0 bytes instead of the expected 1184057 bytes\n: exit status 1" error.file="/go/src/github.com/vmware-tanzu/velero/pkg/restic/backupper.go:179" error.function="github.com/vmware-tanzu/velero/pkg/restic.(*backupper).BackupPodVolumes" logSource="pkg/backup/backup.go:431" name=wikijs-8d87f4fd5-xkqrg
```

If you get this message and you are using minio behind a reverse proxy (nginx in my case), make sure that the ingress allows larger files to be uploaded.

With nginx you can simply add something along the lines of 
```
nginx.ingress.kubernetes.io/proxy-body-size = 1024m
nginx.ingress.kubernetes.io/proxy-request-buffering = off
```
