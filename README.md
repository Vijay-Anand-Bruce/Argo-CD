# Argo Helm Chart

The Argo Helm Chart simplifies the deployment and management of two applications (`app1` and `app2`) on an Amazon EKS cluster using Helm and Argo CD. It provides a scalable, automated setup with ALB-backed Ingress and Horizontal Pod Autoscalers, supporting `dev`, `uat`, and `prod` environments for streamlined CI/CD workflows.

## Overview

This Helm chart deploys two applications (app1 and app2) into a Kubernetes cluster using EKS and Helm. It includes:

- Two Deployments
- Two Services
- Two Horizontal Pod Autoscalers (HPAs)
- One ALB-backed Ingress (shared)
- Argo CD integration for dev, uat, and prod environments

## Project Structure

```
/argo/
├── templates/
│   ├── deployment-1.yaml
│   ├── deployment-2.yaml
│   ├── service-1.yaml
│   ├── service-2.yaml
│   ├── ingress.yaml
│   ├── hpa-1.yaml
│   └── hpa-2.yaml
├── Chart.yaml
├── dev-values.yaml
├── uat-values.yaml
├── prod-values.yaml
└── argo/
    ├── dev-application.yaml
    ├── uat-application.yaml
    └── prod-application.yaml
```

## Prerequisites

- Helm
- Argo CD installed and configured in your cluster
- AWS Load Balancer Controller installed
- IAM OIDC provider enabled for EKS
- A valid ACM certificate ARN
- Git repository accessible by Argo CD

## Manual Installation (using Helm CLI)

You can install the chart directly using Helm:

### Dev
```bash
helm install argo-dev ./manifest -f dev-values.yaml
```

### UAT
```bash
helm install argo-uat ./manifest -f uat-values.yaml
```

### Prod
```bash
helm install argo-prod ./manifest -f prod-values.yaml
```

## Argo CD Deployment

Each environment is managed via its own Argo CD Application manifest.

Replace `dev-values.yaml` with `uat-values.yaml` and `prod-values.yaml` in the respective Argo CD application manifests.

Apply the Application manifest:
```bash
kubectl apply -f argo/applications/dev-application.yaml
kubectl apply -f argo/applications/uat-application.yaml
kubectl apply -f argo/applications/prod-application.yaml
```

## Upgrade Using Helm (optional)

```bash
helm upgrade argo-dev ./argo -f dev-values.yaml
```

## Uninstall

```bash
helm uninstall argo-dev
```