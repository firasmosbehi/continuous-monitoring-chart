# Continuous Monitoring Helm Chart

Deploy a production-ready monitoring stack with one Helm command. The chart bundles:
- Grafana (dashboards + pre-provisioned datasources)
- Prometheus (metrics)
- Loki + Promtail (logs)
- Node Exporter + kube-state-metrics (cluster visibility)
- Optional sample Postgres and MySQL deployments with built-in exporters for metrics validation

Defaults are safe, images are pinned by digest, and every component can be toggled via values.

## Quickstart
```sh
helm repo add continuous-monitoring https://firasmosbehi.github.io/continuous-monitoring-chart
helm install continuous-monitoring continuous-monitoring/continuous-monitoring
```
Access Grafana locally:
```sh
kubectl port-forward svc/continuous-monitoring-continuous-monitoring-grafana 3000:3000
open http://localhost:3000
```
Default credentials: `admin/admin`.

## Default components and versions
- Grafana `12.3.1`
- Prometheus `3.5.1`
- Loki `3.6.4`
- Promtail `3.6.4`
- Node Exporter `master` (current digest with CVE fixes)
- kube-state-metrics `2.18.0`
- Optional: Postgres `17.2-alpine` + postgres-exporter `v0.16.0`; MySQL `8.4` + mysqld-exporter `v0.15.0`

All images are pinned by digest for reproducibility and to avoid pulling vulnerable variants.

## Configure the chart
- Enable/disable components: `grafana.enabled`, `datasources.prometheus.enabled`, `datasources.loki.enabled`, `logs.promtail.enabled`, `exporters.nodeExporter.enabled`, `exporters.kubeStateMetrics.enabled`, `dashboards.enabled`.
- Override images: `.image.repository`, `.image.tag`, `.image.digest` per component.
- Resource tuning: `resources`, `nodeSelector`, `affinity`, `tolerations` per workload.
- Loki URL override: `datasources.loki.url` or `logs.promtail.clientUrl`.
- Spin up sample Postgres with metrics: `--set databases.postgres.enabled=true`
- Spin up sample MySQL with metrics: `--set databases.mysql.enabled=true`

See the full values table in `charts/continuous-monitoring/README.md`.

## Validate locally
- Lint: `helm lint charts/continuous-monitoring`
- Render manifests: `helm template continuous-monitoring charts/continuous-monitoring`

## Release workflow
1) Bump chart version in `charts/continuous-monitoring/Chart.yaml`.  
2) `helm lint charts/continuous-monitoring`  
3) `helm package charts/continuous-monitoring`  
4) `helm repo index .` (for GitHub Pages/S3 hosting)  
5) Publish the `.tgz` and `index.yaml`; keep `artifacthub-repo.yml` in sync.

## Troubleshooting
- Helm upgrade stuck: `helm history continuous-monitoring` then `helm rollback <rev>`.
- Cluster reachability issues: `kubectl cluster-info` and kubeconfig context checks.
- Dashboards missing: confirm `dashboards.enabled=true` and the related exporter is enabled.

## Project layout
- `charts/continuous-monitoring/`: Helm chart source (Chart.yaml, templates, values, README).
- `artifacthub-repo.yml`: Artifact Hub repository metadata.

## Helm repo URL
`https://firasmosbehi.github.io/continuous-monitoring-chart`

## License
Apache-2.0
