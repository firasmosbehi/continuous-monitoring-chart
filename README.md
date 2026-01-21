# Continuous Monitoring Helm Chart

Deploy a production-ready monitoring stack with a single Helm install. This chart bundles Grafana for visualization, Prometheus for metrics scraping, Loki for log aggregation, Promtail for log collection, and exporters for node and Kubernetes state metrics. It is designed for quick cluster observability with sensible defaults and clear extension points.

## What you get
- Grafana pre-provisioned with Prometheus and Loki datasources.
- Node exporter and kube-state-metrics for cluster and node visibility.
- Promtail shipping pod logs to Loki with curated logs dashboards.
- Curated dashboards gated by exporter enablement.
- Simple, values-driven configuration for turning components on or off.

## Repository layout
- `charts/continuous-monitoring/`: Helm chart source (templates, values, chart metadata).
- `artifacthub-repo.yml`: Artifact Hub repository metadata.

## Quickstart
```sh
helm repo add continuous-monitoring https://firasmosbehi.github.io/continuous-monitoring-chart
helm install continuous-monitoring continuous-monitoring/continuous-monitoring
```

Grafana local access:
```sh
kubectl port-forward svc/continuous-monitoring-continuous-monitoring-grafana 3000:3000
```
Open `http://localhost:3000` (default credentials: admin/admin).

## Local development
- Lint the chart: `helm lint charts/continuous-monitoring`
- Render manifests: `helm template continuous-monitoring charts/continuous-monitoring`

## Release workflow
- Package the chart: `helm package charts/continuous-monitoring`
- Update repo index: `helm repo index .`
- Upload the `.tgz` and `index.yaml` to your chart hosting (GitHub Pages, S3, etc.).
- Keep `artifacthub-repo.yml` updated when repository metadata changes.

## Documentation
For install and configuration details, see `charts/continuous-monitoring/README.md`.

## Architecture overview
- Grafana provides dashboards and datasources.
- Prometheus scrapes node-exporter and kube-state-metrics.
- Loki provides log aggregation when enabled.
- Dashboards are provisioned only if their exporters are enabled.

## CI details
- CI packages the chart and publishes releases to the chart repository.
- Artifact Hub metadata is maintained in `artifacthub-repo.yml`.

## Release checklist
- Bump chart version in `charts/continuous-monitoring/Chart.yaml`.
- Validate the chart: `helm lint charts/continuous-monitoring`.
- Package: `helm package charts/continuous-monitoring`.
- Update repo index: `helm repo index .`.
- Publish `.tgz` and `index.yaml` to the hosting location.
- Verify installation from the published repo.

## Troubleshooting
- Helm stuck in pending upgrade: check `helm history continuous-monitoring`, then rollback a known-good revision.
- Cluster unreachable: verify context and DNS with `kubectl cluster-info` and confirm kubeconfig points to the target cluster.

## Helm repo URL
`https://firasmosbehi.github.io/continuous-monitoring-chart`

## License
Apache-2.0
