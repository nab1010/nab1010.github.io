---
layout: post
title: "Deploy Private Docker Registry on Kubernetes"
subtitle: "Kubernetes"
author: "nabang1010"
header-img: "img/in-post/2022/Dec/nvidia_wallpaper.jpg"
# header-style: text
header-mask: 0.4
lang: vi
catalog: true
# hidden: false
section: Knowledge
seo-keywords:
  - kubernetes
  - docker
  - registry
  - private-registry
tags:
  - Docker
  - Kubernetes
---


## Requirements:
    - Kubernetes Cluster
    - Docker

## Step 1: Create Authentication files

Folder tree structure

```
registry/
├── auth
│   └── htpasswd
└── certs
    ├── tls.crt
    └── tls.key
```
### Create and move to `registry` folder

```bash
mkdir registry
cd registry/
```
### Create TLS certificates using ` openssl`

```bash
openssl req -x509 -newkey rsa:4096 -days 365 -nodes -sha256 -keyout certs/tls.key -out certs/tls.crt -subj "/CN=nabang1010-docker-registry" -addext "subjectAltName = DNS:nabang1010-docker-registry"
```
Change `nabang1010-docker-registry` to your domain name

### Create User Authentication using `htpasswd`

Create `auth` folder

```bash
mkdir auth
```

### Use `htpasswd` to create user authentication

```bash
docker run --rm --entrypoint htpasswd registry:2.8.2 -Bbn myuser mypasswd > auth/htpasswd
```
Change `myuser` and `mypasswd` to your username and password

## Step 2: Create Kubernetes Secret to mount the certificates and authentication files

### Secret for TLS certificates

```bash
kubectl create secret tls certs-secret --cert=./registry/certs/tls.crt --key=./registry/certs/tls.key
```
### Secret for Authentication

```bash
kubectl create secret generic auth-secret --from-file=./registry/auth/htpasswd
```
## Step 3: Create Persistent Volume and Persistent Volume Claim

### Prepare `repository-volume.yaml` file


```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: docker-repo-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
  - ReadWriteOnce
  hostPath:
    path: /tmp/repository
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: docker-repo-pvc
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```


```bash

```

```bash
```

```bash
```

```bash
```

```bash
```







