---
title: Nginx Ingress with Rancher
parent: Stuff
grand_parent: Kubernetes
tags: ingress, kubernetes, nginx

---

# nginx ingress annotations


## Allow larger file upload
```
nginx.ingress.kubernetes.io/proxy-body-size = 1024m 
nginx.ingress.kubernetes.io/proxy-request-buffering = off
```


## Ingress class
```
kubernetes.io/ingress.class = "className"
```

##  Nginx https backend 
```
nginx.ingress.kubernetes.io/backend-protocol: HTTPS
```
