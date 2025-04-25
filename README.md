# Homelab Cluster

A Kubernetes-based homelab cluster running on k3s, managed with ArgoCD for GitOps deployments using the App of Apps pattern.

## Overview

This repository contains the configuration for a Kubernetes homelab cluster, using ArgoCD for continuous deployment and GitOps practices. The cluster is set up with various self-hosted applications and services, running on k3s - a lightweight Kubernetes distribution perfect for homelab environments.

The repository uses the App of Apps pattern for managing applications, providing a clear and maintainable structure for application deployment and management.

## Architecture

The cluster is bootstrapped using ArgoCD, which manages the deployment of all applications through GitOps principles. The main components include:

- **k3s**: Lightweight Kubernetes distribution
- **ArgoCD**: For GitOps-based continuous deployment
- **Ingress-NGINX**: For ingress management and routing
- **Cloudflared**: For secure tunnel access
- **Uptime-Kuma**: For monitoring and uptime tracking
- **Linkding**: For bookmark management
- **Whoami**: A simple service for testing and debugging

## Directory Structure

```
.
├── apps/                       # Application configurations
│   ├── applications/          # Individual ArgoCD Application manifests
│   │   ├── ingress-nginx-app.yaml
│   │   ├── linkding-app.yaml
│   │   ├── uptime-kuma-app.yaml
│   │   ├── whoami-app.yaml
│   │   └── cloudflared-app.yaml
│   ├── manifests/             # Application manifests
│   │   ├── ingress-nginx/     # Ingress controller setup
│   │   ├── linkding/         # Bookmark manager
│   │   ├── uptime-kuma/      # Monitoring solution
│   │   ├── whoami/           # Testing service
│   │   └── cloudflared/      # Cloudflare Tunnel configuration
│   └── root-application.yaml  # Root Application
└── bootstrap/                 # Cluster bootstrap configurations
    ├── 00-argocd-install.yaml    # ArgoCD installation manifests
    └── 01-argocd-bootstrap-app.yaml  # Bootstrap application pointing to root
```

## Bootstrap Process

The bootstrap process consists of two main steps:

1. **ArgoCD Installation** (`00-argocd-install.yaml`):
   - Contains all necessary manifests to install ArgoCD
   - Includes CRDs, deployments, services, and other required resources
   - Applied first to set up the ArgoCD infrastructure

2. **Bootstrap Application** (`01-argocd-bootstrap-app.yaml`):
   - Creates the initial ArgoCD Application
   - Points to the root application in the apps directory
   - Triggers the deployment of all other applications through the App of Apps pattern

## Applications

### Core Infrastructure
- **k3s**: Lightweight Kubernetes distribution optimized for resource-constrained environments
- **ArgoCD**: GitOps continuous delivery tool
- **Ingress-NGINX**: Kubernetes ingress controller
- **Cloudflared**: Cloudflare Tunnel for secure access

### Self-hosted Services
- **Uptime-Kuma**: Monitoring and uptime tracking
- **Linkding**: Bookmark manager
- **Whoami**: Simple service for testing and debugging

## Getting Started

### Prerequisites
- k3s installed and running
- kubectl configured to access the cluster
- ArgoCD CLI (optional)

### k3s Installation
If you haven't installed k3s yet, you can install it using:

```bash
curl -sfL https://get.k3s.io | sh -
```

For more installation options and configurations, refer to the [k3s documentation](https://docs.k3s.io/).

### Cluster Setup

1. Apply the ArgoCD bootstrap configuration:
   ```bash
   kubectl apply -f bootstrap/00-argocd-install.yaml
   ```

2. Apply the ArgoCD application configuration:
   ```bash
   kubectl apply -f bootstrap/01-argocd-bootstrap-app.yaml
   ```

3. Access ArgoCD UI (default credentials: admin/password):
   ```bash
   kubectl port-forward svc/argocd-server -n argocd 8080:443
   ```
   Then open https://localhost:8080 in your browser

4. Get the initial ArgoCD admin password:
   ```bash
   kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
   ```

## Management

All applications are managed through ArgoCD using the App of Apps pattern. The structure is as follows:

1. **Root Application**: `apps/root-application.yaml`
   - Manages all individual applications
   - Points to the `apps/applications` directory

2. **Individual Applications**: Located in `apps/applications/`
   - Each application has its own Application manifest
   - References the corresponding manifests in `apps/manifests/`

3. **Application Manifests**: Located in `apps/manifests/`
   - Contains the actual Kubernetes manifests for each application
   - Organized by application name

### Common Operations

- **View ArgoCD Applications**:
  ```bash
  kubectl get applications -n argocd
  ```

- **Check Application Status**:
  ```bash
  kubectl get applications -n argocd -o wide
  ```

- **View Application Logs**:
  ```bash
  kubectl logs -n argocd -l app.kubernetes.io/name=argocd-application-controller
  ```

## Adding New Applications

To add a new application:

1. Create a new directory in `apps/manifests/` for your application
2. Add the application manifests to the new directory
3. Create a new Application manifest in `apps/applications/`
4. Commit and push the changes
5. ArgoCD will automatically sync the new application

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details. 