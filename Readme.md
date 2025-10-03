# My-HomeLab Kubernetes Manifests

This repository contains Kubernetes manifests for deploying a personal homelab stack, including blogging, portfolio, photo management, and database services.

## Structure

```
kube/
  config/      # ConfigMaps for service configuration
  ingress/     # Ingress resources for routing
  resources/   # Deployments, Services, PersistentVolumes, etc.
    db/        # Database resources (MySQL, Postgres, Redis)
    ghost/     # Ghost blog resources
    immich/    # Immich photo management resources
    portfolio/ # Portfolio app resources
  secret/      # Secrets for sensitive configuration
```

## Services

- **Ghost Blog**  
  - Config: [`kube/config/ghost.yaml`](kube/config/ghost.yaml)  
  - Secrets: [`kube/secret/ghost.yaml`](kube/secret/ghost.yaml)  
  - Deployment: [`kube/resources/ghost/server.yaml`](kube/resources/ghost/server.yaml)  
  - Storage: [`kube/resources/ghost/storage.yaml`](kube/resources/ghost/storage.yaml)  
  - Ingress: [`kube/ingress/ingress-blog.yaml`](kube/ingress/ingress-blog.yaml)

- **Portfolio App**  
  - Config: [`kube/config/portfolio.yaml`](kube/config/portfolio.yaml)  
  - Secrets: [`kube/secret/portfolio.yaml`](kube/secret/portfolio.yaml)  
  - API Deployment: [`kube/resources/portfolio/server.yaml`](kube/resources/portfolio/server.yaml)  
  - UI Deployment: [`kube/resources/portfolio/ui.yaml`](kube/resources/portfolio/ui.yaml)  
  - Ingress: [`kube/ingress/ingress-portfolio.yaml`](kube/ingress/ingress-portfolio.yaml)

- **Immich (Photo Management)**  
  - Config: [`kube/config/immich.yaml`](kube/config/immich.yaml)  
  - Secrets: [`kube/secret/immich.yaml`](kube/secret/immich.yaml)  
  - Server Deployment: [`kube/resources/immich/server.yaml`](kube/resources/immich/server.yaml)  
  - ML Deployment: [`kube/resources/immich/ml.yaml`](kube/resources/immich/ml.yaml)  
  - Storage: [`kube/resources/immich/storage.yaml`](kube/resources/immich/storage.yaml)  
  - Ingress: [`kube/ingress/ingress-immich.yaml`](kube/ingress/ingress-immich.yaml)

- **Databases**
  - **MySQL**  
    - Deployment: [`kube/resources/db/mysql/server.yaml`](kube/resources/db/mysql/server.yaml)  
    - Storage: [`kube/resources/db/mysql/storage.yaml`](kube/resources/db/mysql/storage.yaml)  
    - Secrets: [`kube/secret/mysql.yaml`](kube/secret/mysql.yaml)
  - **Postgres**  
    - Deployment: [`kube/resources/db/postress/server.yaml`](kube/resources/db/postress/server.yaml)  
    - Storage: [`kube/resources/db/postress/storage.yaml`](kube/resources/db/postress/storage.yaml)  
    - Secrets: [`kube/secret/postgress.yaml`](kube/secret/postgress.yaml)
  - **Redis**  
    - Deployment: [`kube/resources/db/redis/server.yaml`](kube/resources/db/redis/server.yaml)

## Usage

1. **Configure NFS/hostPath storage** as referenced in the PersistentVolume manifests.
2. **Apply secrets and configmaps**:
   ```sh
   kubectl apply -f kube/secret/
   kubectl apply -f kube/config/
   ```
3. **Deploy resources**:
   ```sh
   kubectl apply -f kube/resources/
   ```
4. **Set up ingress** (ensure your cluster has an ingress controller):
   ```sh
   kubectl apply -f kube/ingress/
   ```

## Notes

- Update NFS server addresses and paths in PV manifests as needed.
- Secrets are base64 encoded.
- Namespace usage:  
  - `blog` for Ghost  
  - `portfolio` for Portfolio  
  - `immich` for Immich  
  - `DB` for databases

## License

MIT