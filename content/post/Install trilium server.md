---
title: Install Trilium server and config TLS with Let's Encrypt
date: 2019-06-04 
lastmod: 2019-06-04
tags: 
    - Server
    - Tips
    - HTTPS
    - TLS
    - Trilium
categories: 
    - 2019-06
url: 
    /tips/trilium-server

description: "install trilium server and set https"

---

> OS: CentOS7     
> Let's Encrypt Client: Certbot       
> Package: trilium-server

# Certbot
1. Enable the EPEL repository
```
$ yum -y install yum-utils
$ yum-config-manager --enable rhui-REGION-rhel-server-extras rhui-REGION-rhel-server-optional
```
2. Installation
```
$ sudo yum install certbot
```

# TLS
1. Config
```
$ sudo certbot certonly --standalone -d baddate.cn -d www.baddate.cn
```
2. Automating renewal

refer to this [official article](https://certbot.eff.org/docs/using.html#renewal)
# Trilium
1. modify `~/trilium-data/config.ini` with `vim`
```
[General]
# Instance name can be used to distinguish between different instances
instanceName=

[Network]
# port setting is relevant only for web deployments, desktop builds run on random free port
port=8080
# true for TLS/SSL/HTTPS (secure), false for HTTP (unsecure).
https=true 
# path to certificate (run "bash bin/generate-cert.sh" to generate self-signed certificate). Relevant only if https=true
certPath=/etc/letsencrypt/live/[example.com]/fullchain.pem
keyPath=/etc/letsencrypt/live/[example.com]/privkey.pem
```
