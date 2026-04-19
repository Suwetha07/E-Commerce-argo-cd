# E-Commerce ArgoCD GitOps Repository

This repository contains **all Kubernetes deployment manifests** for the E-Commerce microservices platform, managed via **ArgoCD** and **Helm**.

## Architecture

```
E-Commerce-ArgoCD/
├── apps/                          # ArgoCD Application manifests
│   ├── app-of-apps.yaml          # Master app (apply this ONCE)
│   ├── clahan-app.yaml           # Deploys to 'clahan' namespace
│   └── clahanstore-app.yaml      # Deploys to 'clahanstore' namespace
├── helm-chart/                    # Umbrella Helm chart
│   ├── Chart.yaml
│   ├── values.yaml               # Default values (clahanstore)
│   ├── values-clahan.yaml        # Override for clahan namespace
│   ├── templates/                 # Shared templates (ConfigMap, Secret, etc.)
│   └── charts/                    # Sub-charts for each microservice
│       ├── api-gateway/
│       ├── frontend-ui/
│       ├── user-service/
│       ├── auth-service/
│       ├── product-service/
│       ├── search-service/
│       ├── inventory-service/
│       ├── cart-service/
│       ├── wishlist-service/
│       ├── order-service/
│       ├── payment-service/
│       ├── invoice-service/
│       ├── shipping-service/
│       ├── review-rating-service/
│       ├── recommendation-service/
│       ├── admin-service/
│       └── mongodb/
└── README.md
```

## Quick Start

```bash
# 1. Install ArgoCD (if not already installed)
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# 2. Apply the App-of-Apps (only do this ONCE)
kubectl apply -f apps/app-of-apps.yaml

# 3. Check ArgoCD dashboard
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

## Namespaces

| Namespace | Purpose | ArgoCD App |
|-----------|---------|------------|
| `clahan` | New deployment | `clahan-ecommerce` |
| `clahanstore` | Existing deployment | `clahanstore-ecommerce` |

## Helm Validation

```bash
# Lint
helm lint helm-chart/

# Template for clahan
helm template clahan helm-chart/ -f helm-chart/values-clahan.yaml

# Template for clahanstore
helm template clahanstore helm-chart/
```

## Microservices

| Service | Port | Type |
|---------|------|------|
| frontend-ui | 80 | Deployment |
| api-gateway | 3000 | Deployment |
| user-service | 3001 | StatefulSet |
| auth-service | 3002 | StatefulSet |
| product-service | 3003 | StatefulSet |
| search-service | 3004 | StatefulSet |
| inventory-service | 3005 | StatefulSet |
| cart-service | 3006 | StatefulSet |
| wishlist-service | 3007 | StatefulSet |
| order-service | 3008 | StatefulSet |
| payment-service | 3009 | StatefulSet |
| invoice-service | 3010 | StatefulSet |
| shipping-service | 3011 | StatefulSet |
| review-rating-service | 3012 | StatefulSet |
| recommendation-service | 3013 | StatefulSet |
| admin-service | 3014 | StatefulSet |
| mongodb | 27017 | StatefulSet |
