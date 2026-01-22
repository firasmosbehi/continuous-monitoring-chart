# continuous-monitoring

Helm chart for deploying the Continuous Monitoring stack.

## Prerequisites
- Kubernetes 1.20+
- Helm 3.8+

## Install
```sh
helm repo add continuous-monitoring https://firasmosbehi.github.io/continuous-monitoring-chart
helm install continuous-monitoring continuous-monitoring/continuous-monitoring
```

## Upgrade
```sh
helm upgrade continuous-monitoring continuous-monitoring/continuous-monitoring
```

## Uninstall
```sh
helm uninstall continuous-monitoring
```

## Quick access
Grafana is deployed with default credentials (admin/admin). To access it locally:
```sh
kubectl port-forward svc/continuous-monitoring-continuous-monitoring-grafana 3000:3000
```
Then open `http://localhost:3000`.

## Feature overview
- Grafana with pre-provisioned datasources (Prometheus, Loki) when enabled.
- Prometheus scrape config includes node-exporter and kube-state-metrics targets.
- Dashboards are provisioned only when their corresponding exporters are enabled.
- Promtail ships pod logs to Loki and provisions a logs dashboard with severity (error/warning/normal) and free-text filters.

## Configuration
Key values in `values.yaml`:
- `exporters.nodeExporter.enabled`: Deploy node-exporter and its dashboard.
- `exporters.kubeStateMetrics.enabled`: Deploy kube-state-metrics and its dashboard.
- `grafana.enabled`: Enable Grafana and dashboard provisioning.
- `datasources.prometheus.enabled`: Enable Prometheus datasource and deployment.
- `datasources.loki.enabled`: Enable Loki datasource and deployment.
- `datasources.loki.url`: Override the Loki datasource URL (defaults to in-cluster service).
- `logs.promtail.enabled`: Enable log collection via Promtail.
- `dashboards.enabled`: Master switch for dashboards (still gated by exporter flags).

Example:
```sh
helm install continuous-monitoring \
  continuous-monitoring/continuous-monitoring \
  --set exporters.nodeExporter.enabled=false
```

## Values
| Key | Type | Default | Description |
| --- | ---- | ------- | ----------- |
| nameOverride | string | `""` | Override the chart name. |
| fullnameOverride | string | `""` | Override the release name. |
| exporters.nodeExporter.enabled | bool | `true` | Enable node-exporter. |
| exporters.nodeExporter.image.repository | string | `"quay.io/prometheus/node-exporter"` | Node-exporter image repository. |
| exporters.nodeExporter.image.tag | string | `"v1.7.0"` | Node-exporter image tag. |
| exporters.nodeExporter.image.pullPolicy | string | `"IfNotPresent"` | Node-exporter image pull policy. |
| exporters.nodeExporter.resources | object | `{}` | Node-exporter resource requests/limits. |
| exporters.nodeExporter.nodeSelector | object | `{}` | Node-exporter node selector. |
| exporters.nodeExporter.affinity | object | `{}` | Node-exporter affinity rules. |
| exporters.nodeExporter.tolerations | list | `[]` | Node-exporter tolerations. |
| exporters.kubeStateMetrics.enabled | bool | `true` | Enable kube-state-metrics. |
| exporters.kubeStateMetrics.image.repository | string | `"registry.k8s.io/kube-state-metrics/kube-state-metrics"` | Kube-state-metrics image repository. |
| exporters.kubeStateMetrics.image.tag | string | `"v2.10.1"` | Kube-state-metrics image tag. |
| exporters.kubeStateMetrics.image.pullPolicy | string | `"IfNotPresent"` | Kube-state-metrics image pull policy. |
| exporters.kubeStateMetrics.resources | object | `{}` | Kube-state-metrics resource requests/limits. |
| exporters.kubeStateMetrics.nodeSelector | object | `{}` | Kube-state-metrics node selector. |
| exporters.kubeStateMetrics.affinity | object | `{}` | Kube-state-metrics affinity rules. |
| exporters.kubeStateMetrics.tolerations | list | `[]` | Kube-state-metrics tolerations. |
| grafana.enabled | bool | `true` | Enable Grafana. |
| grafana.image.repository | string | `"grafana/grafana"` | Grafana image repository. |
| grafana.image.tag | string | `"10.4.2"` | Grafana image tag. |
| grafana.image.pullPolicy | string | `"IfNotPresent"` | Grafana image pull policy. |
| grafana.resources | object | `{}` | Grafana resource requests/limits. |
| grafana.nodeSelector | object | `{}` | Grafana node selector. |
| grafana.affinity | object | `{}` | Grafana affinity rules. |
| grafana.tolerations | list | `[]` | Grafana tolerations. |
| datasources.enabled | bool | `true` | Enable datasource provisioning. |
| datasources.prometheus.enabled | bool | `true` | Enable Prometheus datasource and deployment. |
| datasources.prometheus.image.repository | string | `"prom/prometheus"` | Prometheus image repository. |
| datasources.prometheus.image.tag | string | `"v2.50.1"` | Prometheus image tag. |
| datasources.prometheus.image.pullPolicy | string | `"IfNotPresent"` | Prometheus image pull policy. |
| datasources.prometheus.resources | object | `{}` | Prometheus resource requests/limits. |
| datasources.prometheus.nodeSelector | object | `{}` | Prometheus node selector. |
| datasources.prometheus.affinity | object | `{}` | Prometheus affinity rules. |
| datasources.prometheus.tolerations | list | `[]` | Prometheus tolerations. |
| datasources.loki.enabled | bool | `true` | Enable Loki datasource and deployment. |
| datasources.loki.image.repository | string | `"grafana/loki"` | Loki image repository. |
| datasources.loki.image.tag | string | `"2.9.5"` | Loki image tag. |
| datasources.loki.image.pullPolicy | string | `"IfNotPresent"` | Loki image pull policy. |
| datasources.loki.url | string | `""` | Override Loki service URL for the Grafana datasource. |
| datasources.loki.resources | object | `{}` | Loki resource requests/limits. |
| datasources.loki.nodeSelector | object | `{}` | Loki node selector. |
| datasources.loki.affinity | object | `{}` | Loki affinity rules. |
| datasources.loki.tolerations | list | `[]` | Loki tolerations. |
| logs.promtail.enabled | bool | `true` | Enable Promtail log collection. |
| logs.promtail.image.repository | string | `"grafana/promtail"` | Promtail image repository. |
| logs.promtail.image.tag | string | `"2.9.5"` | Promtail image tag. |
| logs.promtail.image.pullPolicy | string | `"IfNotPresent"` | Promtail image pull policy. |
| logs.promtail.clientUrl | string | `""` | Override Loki client URL. |
| logs.promtail.resources | object | `{}` | Promtail resource requests/limits. |
| logs.promtail.nodeSelector | object | `{}` | Promtail node selector. |
| logs.promtail.affinity | object | `{}` | Promtail affinity rules. |
| logs.promtail.tolerations | list | `[]` | Promtail tolerations. |
| dashboards.enabled | bool | `true` | Enable dashboard provisioning. |

## Troubleshooting
- Grafana CrashLoop on dashboard provisioning: check logs with `kubectl logs deploy/continuous-monitoring-continuous-monitoring-grafana` and confirm dashboard provider configmaps exist.
- Dashboards missing: ensure `dashboards.enabled=true` and the corresponding exporter is enabled; check with `kubectl get configmaps | rg dashboard`.
- Helm stuck in pending upgrade: inspect with `helm history continuous-monitoring` and rollback the last successful revision, for example `helm rollback continuous-monitoring 1`.
- Cluster unreachable errors: verify context and DNS using `kubectl cluster-info` and confirm the kubeconfig points to the expected cluster.
