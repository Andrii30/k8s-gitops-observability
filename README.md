# k8s-gitops-observability

GitOps for cluster infrastructure: kube-prometheus-stack + Loki + Uptime
Kuma, all deployed and reconciled by one ArgoCD app-of-apps.

## Prerequisites
- A Kubernetes cluster with ArgoCD installed (shared setup, once per
  cluster).

## Deploy everything
```bash
kubectl apply -f apps/root-app.yaml
```
That one command bootstraps ArgoCD to also adopt every other manifest
under `apps/` — kube-prometheus-stack, Loki, Uptime Kuma, and the
custom dashboard — automatically.

## Access
```bash
kubectl -n observability port-forward svc/kube-prometheus-stack-grafana 3002:80
kubectl -n observability port-forward svc/uptime-kuma 3001:3001
```
Grafana: http://localhost:3002 (`admin` / `ChangeMe123!`)
Uptime Kuma: http://localhost:3001

## GitOps flow
Add a new Application manifest under `apps/`, push — ArgoCD's
`directory.recurse` root app picks it up with no manual `kubectl apply`.
