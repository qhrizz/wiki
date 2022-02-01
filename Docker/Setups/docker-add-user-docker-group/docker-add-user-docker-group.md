---
title: Add user to docker group
description: 
published: 1
tags: docker
parent: Setups
grand_parent: Docker
---

# Add user to Docker group
This will enable a user to interact with the docker service 

```
sudo usermod -aG docker $USER$ && newgrp docker
```