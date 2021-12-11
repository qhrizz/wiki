---
title: Velero with minio backend
parent: Setups
grand_parent: Kubernetes
tags: backup, kubernetes, minio, velero

---

# Velero with minio backend
## Minio

Since Velero only likes objectstorage backend we will utilize minio on prem to send our data to.
Please see the official instruction on how to deply: https://docs.min.io/docs/deploy-minio-on-docker-compose.html (set a good password, duh)
You will also need to publish the service with a valid certificate (perhaps other ways but I had trouble using selfsigned/no cert)


## Velero 
Velero consists of two components, cli and “server” (deployment workload on the cluster). To accompany this you will also need kubectl and the kubectl config from your cluster

Install the Velero CLI on whatever platform you use using: https://velero.io/docs/v1.5/basic-install/
Minio uses the AWS backend from Velero so to deploy it in our cluster

Create a credentials-velero file containing

```
[default]
aws_access_key_id=YOUR_ACCESS_KEY
aws_secret_access_key=YOUR_SECRET_KEY
```

https://github.com/vmware-tanzu/velero-plugin-for-aws Adjust velero-plugin-for-aws:v1.1.0 according to the compability chart
To install Velero in your cluster run: 
```
velero install  --provider aws --bucket default \
--plugins velero/velero-plugin-for-aws:v1.1.0 \
--secret-file ./credentials-velero \
--use-volume-snapshots=false \
--use-restic \
--backup-location-config \
region=minio,s3ForcePathStyle="true",s3Url=https://minio.qhrizz.se,publicUrl=https://minio.qhrizz.se
```

This will provison velero and xWorkers restic helper which handles the file backup. 

```
kubectl.exe -n velero get pod
NAME                      READY   STATUS    RESTARTS   AGE
restic-d52sr              1/1     Running   0          21h
restic-kz4x2              1/1     Running   0          21h
restic-xzc2d              1/1     Running   0          21h
velero-757d76d6b8-4r8pl   1/1     Running   2          21h
```

The next step is to create the bucket in minio. Sign in to minio and simply press the little button in the lower right and create bucket

The next step is to create a default storage location
```
velero backup-location create teamspeak --bucket teamspeak  --provider aws --config region=minio,s3ForcePathStyle="true",s3Url=https://minio.qhrizz.se,publicUrl=https://minio.qhrizz.se
```

This will create a backup location called “teamspeak”. You can verify with
`velero backup-location get`


Which should return your location

```
PS C:\Users\qhrizz> velero.exe backup-location get
NAME               PROVIDER   BUCKET/PREFIX      PHASE       LAST VALIDATED                   ACCESS MODE
teamspeak          aws        teamspeak          Available   2020-10-23 18:47:30 +0200 CEST   ReadWrite
```

To make velero backup the persistent storage you need to annotate your deployment

`backup.velero.io/backup-volumes = [Volume-Name]`

Then create a daily backupjob
`velero schedule create teamspeak --schedule="@daily" --include-namespaces teamspeak --storage-location teamspeak`

## Multiple backup-locations from velero 1.6(?)
Changes has been made to have to create the backup location starting from velero 1.6 (?) 

### Create backup-location with default credentials
This will create a backup-location using the credentials you installed velero with

```
velero backup-location create cl1-wikijs --bucket cl1-wikijs  --provider aws --config region=minio,s3ForcePathStyle="true",s3Url=https://minio.qhrizz.se,publicUrl=https://minio.qhrizz.se --credential cloud-credentials=cloud
```

Take note of **--credential cloud-credentials=cloud** 
This is added to point to the secret which is used for authentication which can be seen with 

```
kubectl describe secret cloud-credentials -n velero
```

## Different authentication credentials for different backup-locations
This creates a new secret based on the authentication credentials in a file
Create a new file 
```
[default]
aws_access_key_id="myUser"
aws_secret_access_key="myPassword"
```

Create a new secret with
```
kubectl create secret generic -n velero credentials --from-file=cl1-teamspeak=./cl1-teamspeak 
```
This will create a secret called "credentials" with the key cl1-teamspeak and value from the file. 

And then create the backup-location with 
```
velero backup-location create cl1-teamspeak --bucket cl1-teamspeak --provider aws --config region=minio,s3ForcePathStyle="true",s3Url=https://minio.qhrizz.se,publicUrl=https://minio.qhrizz.se --credential credentials=cl1-teamspeak
```
The --credential is pointing to the secretName=KeyName


## Restore

The simplest of restores is to test a “disaster” of accidental removal of a namespace.
Remove the namespace with
`kubectl delete namespace teamspeak`
Wait for it to finish

We can then either restore from the last completed schedule backup or you can specify a backup name from
`velero backup get`
Then run
`velero restore create --from-backup teamspeak-[id number]`
Or restore from the last scheduled backup
`velero restore create --from-schedule teamspeak`


# Advanced restores
You can also restore to another namespace if you want to run it in a separate environment
`velero restore create --from-schedule teamspeak --namespace-mappings old-namespace:new-namespace`
This can however have trouble creating PVCs for the restored deployment

# Restore to another cluster
Install velero the same way in the new cluster
Then create the same backup location in readOnly mode

```
velero backup-location create teamspeak --bucket teamspeak  --provider aws --config region=minio,s3ForcePathStyle="true",s3Url=https://minio.qhrizz.se,publicUrl=https://minio.qhrizz.se --access-mode ReadOnly
```

Verify that you can see the backup 
`velero backup get`

Begin restoring

`velero restore create --from-backup [backup-name]`

(You may have to move the namespace back into the cluster. Go to the Cluster overview -> Project/Namespaces -> Locate the namespace you restored and move into the project)

# Install without default backup-location

```
velero install --plugins velero/velero-plugin-for-aws:v1.1.0 \
--secret-file ./credentials-velero \
--use-volume-snapshots=false \
--use-restic \
--no-default-backup-location
```



