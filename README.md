# WeDevs DevOps Assignment – GitOps Repository

This repository contains the full GitOps configuration for the DevOps assignment, including:

- k3s cluster (created externally, e.g. via k3d)
- FluxCD GitOps bootstrap
- SOPS + age secret encryption for MySQL credentials
- MetalLB (bare-metal LoadBalancer)
- ingress-nginx (Helm, chart 4.11.7) exposed as LoadBalancer
- MySQL (Bitnami Helm chart) with secrets encrypted using SOPS
- WordPress (Bitnami Helm chart) exposed at http://app.local
- Custom ingress-nginx Lua plugin that redirects all 404 responses to https://google.com
- Monitoring stack:
  - kube-prometheus-stack
  - Loki + Promtail
  - Grafana exposed at http://app-monitor.local
  - Dashboard for WordPress pod logs

## Prerequisites

- A running k3s/k3d cluster
- flux CLI
- sops
- age
- kubectl context pointing to the cluster

## Hosts configuration

Add the following entries to your `/etc/hosts` (or Windows equivalent):

```text
127.0.0.1   app.local
127.0.0.1   app-monitor.local
```

## Flux bootstrap (example)

```bash
flux bootstrap github \
  --owner=<your-github-username> \
  --repository=wedevs-devops-assignment \
  --branch=main \
  --path=clusters/wedevs-cluster
```

Once bootstrapped, Flux will reconcile the manifests in this repository and deploy:

- MetalLB
- ingress-nginx
- MySQL
- WordPress
- Monitoring stack (Prometheus, Loki, Grafana)
- Custom Lua plugin behaviour for 404 redirects

> NOTE: The MySQL secret (`apps/mysql/mysql-secret.enc.yaml`) is *example-encrypted* with SOPS.
> In a real deployment, regenerate and re-encrypt the secret with your own `age` key using `sops -e`.
