# Continuous Monitoring Chart

Deploy a full monitoring stack on Kubernetes with one Helm install. This chart bundles Grafana, Prometheus, Loki, Promtail, node-exporter, and kube-state-metrics — all pinned by digest for reproducibility and to keep vulnerable tags out of your cluster.

## Why this chart
- One command to install a proven observability stack.
- Secure defaults: all images pinned by digest, latest CVE fixes applied.
- Values-driven toggles: enable only the components you need.
- Ready-made dashboards and pre-provisioned datasources.

## Quick start
```sh
helm repo add continuous-monitoring https://firasmosbehi.github.io/continuous-monitoring-chart
helm install continuous-monitoring continuous-monitoring/continuous-monitoring
kubectl port-forward svc/continuous-monitoring-continuous-monitoring-grafana 3000:3000
open http://localhost:3000
```
Grafana credentials: `admin/admin`.

## Default versions (pinned)
- Grafana `12.3.1`
- Prometheus `3.5.1`
- Loki `3.6.4`
- Promtail `3.6.4`
- Node Exporter `master` digest (current CVE-free build)
- kube-state-metrics `2.18.0`
- Optional: Postgres `17.2-alpine` + postgres-exporter `v0.16.0`; MySQL `8.4` + mysqld-exporter `v0.15.0`

## Common switches
- Disable node-exporter: `--set exporters.nodeExporter.enabled=false`
- Use external Loki: `--set datasources.loki.url=http://loki.example.com`
- Logs off: `--set logs.promtail.enabled=false --set datasources.loki.enabled=false`
- Override an image: `--set grafana.image.tag=12.3.1 --set grafana.image.digest=<digest>`
- Sample Postgres with metrics: `--set databases.postgres.enabled=true`
- Sample MySQL with metrics: `--set databases.mysql.enabled=true`

## Operations
- Lint: `helm lint charts/continuous-monitoring`
- Render: `helm template continuous-monitoring charts/continuous-monitoring`
- Upgrade: `helm upgrade continuous-monitoring continuous-monitoring/continuous-monitoring`
- Remove: `helm uninstall continuous-monitoring`

## Security posture
- Images pinned by digest across all components.
- Latest Trivy scan (HIGH/CRITICAL, ignore-unfixed) is clean for defaults, including node-exporter.
- Keep the chart current to inherit upstream security fixes.

## Troubleshooting
- Grafana CrashLoop: `kubectl logs deploy/continuous-monitoring-continuous-monitoring-grafana`
- Missing dashboards: ensure `dashboards.enabled=true` and exporters are enabled; check configmaps with `kubectl get configmaps | rg dashboard`.
- Helm stuck: `helm history continuous-monitoring` then `helm rollback continuous-monitoring <rev>`.

## Project links
- Chart source: `charts/continuous-monitoring/`
- Artifact Hub: https://artifacthub.io/packages/helm/continuous-monitoring/continuous-monitoring
- Helm repo: https://firasmosbehi.github.io/continuous-monitoring-chart
