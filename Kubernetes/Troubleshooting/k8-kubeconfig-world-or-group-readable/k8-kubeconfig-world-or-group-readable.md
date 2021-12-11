---
title: kubeconfig world or group readable
parent: Troubleshooting
grand_parent: Kubernetes
tags: kubernetes

---

# kubeconfig world or group readable
This happens when the permissions for your config file is incorrect. You will be shown a message similar to 

```
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/chta/.kube/config
WARNING: Kubernetes configuration file is world-readable. This is insecure. Location: /home/chta/.kube/config
```
To correct the permissions
```
chmod go-r ~/.kube/config
```

* Source https://github.com/helm/helm/issues/9115