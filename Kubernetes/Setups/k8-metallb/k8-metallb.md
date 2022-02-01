---
title: Setup Metallb
parent: Setups
grand_parent: Kubernetes
tags: metallb,kubernetes,loadbalancer
---


# Guide on how to setup Metallb

When using Kubernetes onprem you need a load balancer to be able to expose your services. In the cloud you would use the already provided load balancers but on prem you need another way (unless you route the traffic through a cloudprovider)

Metallb can be found here https://metallb.universe.tf/installation/1. Open up your cluster and go to Launch kubectl

2. Copy and paste 
```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.3/manifests/namespace.yaml
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.3/manifests/metallb.yaml
```

**On first install only**
```
kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"
```

3. Go to your default project and go to resource -> workload -> Import yamlCreate your yaml and specify an address range that metallb can take from

```
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 10.130.140.200-10.130.140.210
```
4. Done! One provisioning any service with L4 loadbalancer, an ip will be taken from the pool you specified 
