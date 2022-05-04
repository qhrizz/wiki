---
title: Installing and working with Traefik
description: 
published: 1
tags: docker, docker-compose, traefik
parent: Setups
grand_parent: Docker
---

# Installing with docker-compose
```yml
version: '3.3'
services:
  traefik:
    image: "traefik:v2.6.0"
    container_name: "traefik"
    restart: unless-stopped
    logging:
      driver: "json-file"
      options:
        max-size: 10m
        max-file: "5"
    command:
      - "--log.level=DEBUG"
      - "--api.insecure=true"
     # Letsencrypt cert with cloudflare
      - "--certificatesresolvers.letsencrypt.acme.email=email@email.com"
      - "--certificatesresolvers.letsencrypt.acme.storage=/acme/acme.json"
      - "--certificatesresolvers.letsencrypt.acme.dnsChallenge=true"
      - "--certificatesresolvers.letsencrypt.acme.dnschallenge.provider=cloudflare"
      - "--certificatesresolvers.letsencrypt.acme.dnschallenge.delaybeforecheck=10"
      # Staging, uncomment to use the staging environment. Good for testing :) 
      #- "--certificatesresolvers.letsencrypt.acme.caserver=https://acme-staging-v02.api.letsencrypt.org/directory"        
      # Enable the Trafik dashboard
      - "--api.dashboard=true"
      - "--providers.docker=true"
      - "--providers.docker.exposedbydefault=false"
      - "--entrypoints.web.address=:80"
      - "--entrypoints.websecure.address=:443"
     # Global redirect from http to https
      - "--entrypoints.web.http.redirections.entryPoint.to=websecure"
      - "--entrypoints.web.http.redirections.entryPoint.scheme=https"
      - "--entrypoints.web.http.redirections.entrypoint.permanent=true"
     # Directory where you can add .yml dynamic configuration files
      - "--providers.file.directory=/configuration"
      - "--providers.file.watch=true"
      # Pilot connection, not neccasry, only if you use Traefik Pilot
      #- "--pilot.token="
      # Disable backend ssl check, Global setting if you want. 
      #- "--serverstransport.insecureskipverify=true"
    ports:
      - "80:80"
      - "443:443"
      # 8080 is traefik-dashboard
      - "8080:8080"
    volumes:
      - '/var/run/docker.sock:/var/run/docker.sock:ro'
      - 'traefik-configurations:/configuration'
      - 'acme:/acme'
    environment:
      - "CLOUDFLARE_EMAIL=email@email.com"
      - "CLOUDFLARE_DNS_API_TOKEN=Your-Cloudflare-Or-other-provider-see-docs"
      - TZ=Europe/Stockholm
    network_mode: host
    # This part is to enable traefik to expose the dashboard via itself, not needing to use :8080. Comment out if you want to :)  
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.traefik-dashboard.rule=Host(`dashboard.domain.com`)"
      - "traefik.http.routers.traefik-dashboard.entrypoints=websecure"
      - "traefik.http.routers.traefik-dashboard.tls=true"
      - "traefik.http.services.traefik-dashboard.loadbalancer.server.port=8080"
      - "traefik.http.routers.traefik-dashboard.tls.certresolver=letsencrypt"
      # I use a wildcard cert, so I have to specify which domains to trigger. If you create a new cert per domain, the rows below can be omitted
      - "traefik.http.routers.traefik-dashboard.tls.domains[0].main=domain.com"
      - "traefik.http.routers.traefik-dashboard.tls.domains[0].sans=*.domain.com"      
volumes:
  traefik-configurations:
  acme:
```


# Letsencrypt wildcard
To use wildcard cert, you first need to use DNS challenge. Then you can simply add these labels to your deployment
Of course, replace the routers name with the one you have.
```yml
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.traefik-dashboard.rule=Host(`dashboard.domain.com`)"
      - "traefik.http.routers.traefik-dashboard.entrypoints=websecure"
      - "traefik.http.routers.traefik-dashboard.tls=true"
      - "traefik.http.routers.traefik-dashboard.tls.domains[0].main=domain.com"
      - "traefik.http.routers.traefik-dashboard.tls.domains[0].sans=*.domain.com"
```
If you use the file provider way, you can do it like this.

```yml 
http:
  routers:
    dashboard:
      entrypoints:
        - "websecure"
        - "web"
      rule: "Host(`dashboard.domain.com`)"
      tls:
        certResolver: "letsencrypt"
        domains:
          main: 'domain.com'
          sans: '*.domain.com'        
      service: dashboard
  services:
    dashboard:
      loadBalancer:
        servers:
          - url: "https://10.10.10.10:8080"
```
# Using non-docker backends 
If you have non-docker backends or docker containers on others hosts apart from the traefik one, you can use the file provider. 

I would suggest you enter the shell from the traefik container since the permissions can be fucked up and traefik does not like that.
As you can see in my configuration, I have my dynamic configurations folder mounted in **/configuration**

Traefik shell only has **vi**, so **vi /configuration/dashboard.yml** (for example)

```yml
http:
  routers:
    dashboard:
      entrypoints:
        - "websecure"
        - "web"
      rule: "Host(`dashboard.domain.com`)"
      tls:
        certResolver: "letsencrypt"
        domains:
          main: 'domain.com'
          sans: '*.domain.com'        
      service: dashboard
  services:
    dashboard:
      loadBalancer:
        servers:
          - url: "http://10.10.10.10:8080"
```

So simply change the loadbalancer.servers.url to the IP + port of your non/other docker backend. 

# Using non docker backends with self signed certificate
Some backends use a self signed certificate, for example synology. If you did not disable self signed certificate check globally you can do this in the file provider

```yml
http:
  routers:
    synology:
      entrypoints:
        - "websecure"
        - "web"
      rule: "Host(`synology.domain.com`)"
      tls:
        certResolver: "letsencrypt"
        domains:
          main: 'domain.com'
          sans: '*.domain.com'        
      service: synology
  services:
    synology:
      loadBalancer:
        servers:
          - url: "https://<ip-to-synology>:5001/"
        serversTransport: ignore-self-signed
  serversTransports:
    ignore-self-signed:
      insecureskipverify: true 
```

Notice the last rows, these lines disables the self signed check
```yml
        serversTransport: ignore-self-signed
  serversTransports:
    ignore-self-signed:
      insecureskipverify: true
```