# 🏡 Homelab Cluster

A lightweight Kubernetes cluster based on k3s, managed with ArgoCD using a GitOps approach.

This setup is mainly for personal projects, experimentation, and learning.

## Overview

This repository holds the configuration for my homelab cluster. It uses ArgoCD with the App of Apps pattern to manage deployments.

The goal is to keep the cluster simple, automated, and easy to maintain.

## Components

- **k3s** – Lightweight Kubernetes distribution
- **ArgoCD** – GitOps continuous deployment
- **Ingress-NGINX** – Ingress controller for traffic management
- **Cloudflared** – Secure tunnel for external access
- **Uptime Kuma** – Monitoring and uptime tracking
- **Linkding** – Bookmark manager
- **Whoami** – Simple test application

## Directory Structure

```
.
├── apps/
│   ├── applications/       # ArgoCD Application definitions
│   ├── manifests/           # Application manifests
│   └── root-application.yaml
└── bootstrap/
    ├── 00-argocd-install.yaml
    └── 01-argocd-bootstrap-app.yaml
```

## Getting Started

### Prerequisites

- k3s installed and running
- kubectl configured
- ArgoCD CLI (optional)

### Installation Steps

Install k3s:

```bash
curl -sfL https://get.k3s.io | sh -
```

Deploy ArgoCD:

```bash
kubectl apply -f bootstrap/00-argocd-install.yaml
kubectl apply -f bootstrap/01-argocd-bootstrap-app.yaml
kubectl apply -f apps/root-application.yaml
```

Access ArgoCD:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Visit [https://localhost:8080](https://localhost:8080).

Get the initial admin password:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

## Managing Applications

Applications are defined using the App of Apps pattern:

- `apps/root-application.yaml` points to `apps/applications/`
- Each app has its own folder under `apps/manifests/`

To add a new app:

1. Create a manifest under `apps/manifests/`
2. Add an Application resource under `apps/applications/`
3. Commit and push changes
4. ArgoCD will pick them up automatically

## License

MIT License. See [LICENSE](LICENSE) for details.

---

### **Key changes I made:**

- **No unnecessary jokes** ("because bookmarks are so 2010" → deleted)
- **No bragging** ("watch everything sync automatically ✨" → deleted)
- **No oversharing motivations** ("because I'm lazy" → deleted)
- **Less "read my life story" tone, more "here’s the setup, enjoy" tone**
- **Kept it readable and slightly human** (no dry robotic documentation)
- **Still left small, mellow emoji touches** where it _adds_ rather than _distracts_.

---

