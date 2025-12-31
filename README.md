# kubernetes_wordpress

WordPress running on Kubernetes (Docker Desktop local cluster).

This repository is intended for **local development / learning purposes** and mimics a production-like setup using Kubernetes:

- WordPress (PHP-FPM)
- Nginx (custom Dockerfile)
- MySQL 8
- Ingress (nginx-ingress)

---

## Requirements

- Docker Desktop (Kubernetes enabled)
- kubectl
- Ingress NGINX Controller installed on the cluster

> This repository assumes a **local Kubernetes cluster provided by Docker Desktop**.

---

## Repository structure

- `docker/`  
  Dockerfiles and configuration files  
  - PHP-FPM (WordPress app container)
  - Nginx (reverse proxy)

- `k8s/`  
  Kubernetes manifest files  
  - MySQL
  - WordPress application
  - Ingress
  - Secrets (actual secrets are not committed)

- `wordpress/`  
  WordPress source code (downloaded beforehand and copied into the container)

---

## Local domain

This project uses the following **local domain**:

- `dev-k8s.tai333.cloud`

Add the following entry to `/etc/hosts`:

```txt
127.0.0.1 dev-k8s.tai333.cloud
```

---

## Local Setup
- confirm kubernetes context
```bash
kubectl config current-context
```

- create namespace
```bash
kubectl create namespace dev-tai333-k8s || true
```

- build docker images
```bash
docker build -t wiki-app:latest   -f docker/dev/app/Dockerfile .
docker build -t wiki-nginx:latest -f docker/dev/nginx/Dockerfile .
```

- apply secrets (local only)
```bash
kubectl apply -f k8s/secrets.yaml -n dev-tai333-k8s
```

- apply kubernetes manifests
```bash
kubectl apply -f k8s/mysql.yaml   -n dev-tai333-k8s
kubectl apply -f k8s/app.yaml     -n dev-tai333-k8s
kubectl apply -f k8s/ingress.yaml -n dev-tai333-k8s
```

- check status
```bash
kubectl get pods,svc,ingress -n dev-tai333-k8s
```
