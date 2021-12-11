---
title: Velero cheat sheet
parent: Stuff
grand_parent: Kubernetes
tags: cli, commands, velero

---

# Velero cheat sheet
## Create backup location
```
velero backup-location create wiki-demo --bucket wiki-demo --config region=minio,s3ForcePathStyle=”true”,s3Url=https://minio.qhrizz.se --provider aws
```
## Show backups with label
```
velero backup get --show-labels
```
## Show backups based on labels
```
velero backup get -l velero.io/storage-location=wiki-demo
```
## Remove backups based on labels
```
velero backup delete -l ”velero.io/schedule-name=wiki-demo”
```
## Show Velero logs
``` 
kubectl logs deployment/velero -n velero
```
## Show backup logs 
```
velero backup describe [backup name] –details
```
## Delete backup
```
velero backup delete [backup name] (add --confirm to skip prompt)
```
## Remove backups in deleting state
```
kubectl -n velero delete backup.velero.io [backup name]
```
## Remove all backups
```
kubectl -n velero delete backup.velero.io --all
```
## Remove backup location
```
kubectl -n velero delete backupstoragelocation [backup location name]
```
## Show backup location
```
velero backup-location get
```
## Create restore job from backup
```
velero restore create –from-backup [backup name]
```
## Create scheduled backup every 5 min
```
velero schedule create wiki-demo --schedule=”*/5 * * * *” --storage-location wiki-demo --include-namespaces wiki-demo
```
## Set backup location in read only
```
kubectl patch backupstoragelocation [backup location name] --namespace velero --type merge --patch '{”spec”:{”accessMode”:”ReadOnly”}}'
```
## Restore backup location to read and write
```
kubectl patch backupstoragelocation [backup location name] --namespace velero --type merge --patch '{”spec”:{”accessMode”:”ReadWrite”}}'
```
## Delete backup-location
```
kubectl -n velero delete backupstoragelocation NAME
```
























