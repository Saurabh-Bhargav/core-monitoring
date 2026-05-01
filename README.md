# Observability GitOps stack

This repo is a scaffold for a single-node Kubernetes observability platform with:

- Argo CD for GitOps
- Prometheus + Alertmanager for metrics and email alerting
- Grafana for dashboards
- Loki for logs
- project dashboard folders mounted from this repo
- optional VM-side Promtail and node exporter for your PM2 hosts

## What you must replace

- `https://github.com/<ORG>/<REPO>.git`
- SMTP settings in `charts/prometheus-stack/values.yaml`
- Grafana admin password in `charts/grafana/values.yaml`
- any placeholder project names under `grafana/dashboards/projects/`

## Repo layout

- `argocd/bootstrap` contains the app-of-apps bootstrap manifests
- `argocd/projects` contains the Argo CD project definition
- `charts/prometheus-stack` wraps `kube-prometheus-stack`
- `charts/grafana` wraps `grafana`
- `charts/loki` wraps `loki`
- `monitoring/rules` contains PrometheusRule objects
- `monitoring/scrape` contains sample monitors and scrape config fragments
- `external/vm` contains host-side agents for your non-Kubernetes PM2 hosts

## Deployment order

1. Apply the root Argo CD application.
2. Argo CD creates the project, namespaces, and child applications.
3. The monitoring stack comes up first.
4. Grafana and Loki follow.
5. Alert rules and monitors sync after the CRDs exist.

## Notes

- This is sized for a single node with 8 GB RAM.
- Prometheus retention is intentionally short.
- Loki is configured for monolithic mode with local persistence.
- The Grafana chart uses an init container to clone this repo and mount dashboard files from `grafana/dashboards/projects`.

