# My-HomeLab Kubernetes & Automation

This repository contains everything needed to deploy a personal homelab stack on a Kubernetes cluster (tested with MicroK8s), including NFS storage setup, blogging, portfolio, photo management, and database services. Automation is provided via Ansible.

## Project Structure

```
ansible/
  ansible.yaml      # Ansible playbook for NFS and K8s deployment
kube/
  config/           # ConfigMaps for service configuration
  ingress/          # Ingress resources for routing
  resources/        # Deployments, Services, PersistentVolumes, etc.
    db/             # Database resources (MySQL, Postgres, Redis)
    ghost/          # Ghost blog resources
    immich/         # Immich photo management resources
    portfolio/      # Portfolio app resources
  secret/           # Secrets for sensitive configuration
Readme.md           # This file
```

## Automation & Deployment

- **NFS Storage Setup:**  
  Automated using Ansible (`ansible/ansible.yaml`). Installs NFS server, exports directories, and restarts services.

- **Kubernetes Manifests Deployment:**  
  Ansible applies manifests in the following order for reliability:
  1. Storage (PersistentVolumes, PersistentVolumeClaims)
  2. Secrets
  3. ConfigMaps
  4. All other resources (Deployments, Services, etc.)

- **MicroK8s Support:**  
  All manifests and automation are compatible with MicroK8s. Use `microk8s kubectl` or configure Ansible to use your MicroK8s kubeconfig.

## Services

- **Ghost Blog:**  
  - Config, secrets, deployment, storage, ingress

- **Portfolio App:**  
  - Config, secrets, API/UI deployments, ingress

- **Immich (Photo Management):**  
  - Config, secrets, server/ML deployments, storage, ingress

- **Databases:**  
  - MySQL, Postgres, Redis (with storage and secrets)

## Usage

1. **Edit Ansible Inventory:**  
   Define your `nfs-server` and `k8s-master` hosts.

2. **Run Ansible Playbook:**  
   ```sh
   ansible-playbook ansible/ansible.yaml -i <your-inventory>
   ```

3. **Manual Steps (if not using Ansible):**
   - Apply storage manifests first:
     ```sh
     kubectl apply -f kube/resources/**/storage.yaml
     ```
   - Apply secrets:
     ```sh
     kubectl apply -f kube/secret/
     ```
   - Apply configmaps:
     ```sh
     kubectl apply -f kube/config/
     ```
   - Apply other resources:
     ```sh
     kubectl apply -f kube/resources/
     ```
   - Apply ingress:
     ```sh
     kubectl apply -f kube/ingress/
     ```

## Notes

- Update NFS server addresses and paths in PV manifests as needed.
- Secrets must be base64 encoded.
- Namespace usage:
  - `blog` for Ghost
  - `portfolio` for Portfolio
  - `immich` for Immich
  - `DB` for databases

## Requirements

- MicroK8s or any Kubernetes cluster
- Ansible (with `community.kubernetes` collection)
- NFS server (automated setup via Ansible)

## License

MIT