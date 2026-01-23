# continuous-monitoring

Helm chart for a batteries-included monitoring stack: Grafana, Prometheus, Loki, Promtail, node-exporter, and kube-state-metrics. Images are pinned by digest for reproducibility and to avoid pulling vulnerable variants.

## Requirements
- Kubernetes 1.20+
- Helm 3.8+

## Default images (pinned)
- Grafana `12.3.1`
- Prometheus `3.5.1`
- Loki `3.6.4`
- Promtail `3.6.4`
- Node Exporter `master` digest (current build with CVE fixes)
- kube-state-metrics `2.18.0`

## Install / upgrade / remove
```sh
helm repo add continuous-monitoring https://firasmosbehi.github.io/continuous-monitoring-chart
helm install continuous-monitoring continuous-monitoring/continuous-monitoring
helm upgrade continuous-monitoring continuous-monitoring/continuous-monitoring
helm uninstall continuous-monitoring
```

## Access Grafana
```sh
kubectl port-forward svc/continuous-monitoring-continuous-monitoring-grafana 3000:3000
open http://localhost:3000
```
Default credentials: `admin/admin`.

## Highlights
- Grafana ships with Prometheus and Loki datasources (when enabled).
- Prometheus scrapes node-exporter and kube-state-metrics out of the box.
- Dashboards are gated by the corresponding exporter flags.
- Promtail collects pod logs to Loki with severity filters and free-text search.
- Values-driven: toggle components, override images, and tune resources per component.

## Common customizations
- Disable an exporter: `--set exporters.nodeExporter.enabled=false`
- Use an external Loki: `--set datasources.loki.url=http://loki.example.com`
- Keep Grafana only: disable Prometheus, Loki, and exporters via their `.enabled` flags.
- Override image digests or tags per component using the `image.*` blocks.

## Security posture
- All defaults are pinned by digest.
- Latest Trivy scan (HIGH/CRITICAL, ignore-unfixed) is clean for all defaults including node-exporter `master` digest.
- Keep the chart updated to pick up upstream security fixes.

## Values
Effects of key values in `values.yaml`:
- `nameOverride`, `fullnameOverride`: change chart/release naming.
- `exporters.nodeExporter.*`: controls the node-exporter DaemonSet; disable to skip node metrics and related dashboards; adjust image/digest/pullPolicy/resources/scheduling.
- `exporters.kubeStateMetrics.*`: controls kube-state-metrics Deployment; disable to skip Kubernetes state metrics and dashboards; adjust image/digest/pullPolicy/resources/scheduling.
- `grafana.*`: turns Grafana on/off, sets image/digest/pullPolicy, and configures scheduling/resources for the dashboard/UI layer.
- `datasources.enabled`: master switch for provisioning datasources; if false, Prometheus/Loki deployments and datasource configmaps are skipped.
- `datasources.prometheus.*`: controls the Prometheus Deployment; disable to skip in-cluster Prometheus and its datasource; adjust image/digest/pullPolicy/resources/scheduling.
- `datasources.loki.*`: controls the Loki Deployment and datasource; disable to skip in-cluster Loki; `url` overrides the datasource target; adjust image/digest/pullPolicy/resources/scheduling.
- `logs.promtail.*`: controls the Promtail DaemonSet; disable to stop log collection/shipping; `clientUrl` overrides Loki push target; adjust image/digest/pullPolicy/resources/scheduling.
- `dashboards.enabled`: global toggle for dashboard provisioning (still gated by exporter flags).

| Key | Type | Default | Description |
| --- | ---- | ------- | ----------- |
| nameOverride | string | `""` | Override the chart name. |
| fullnameOverride | string | `""` | Override the release name. |
| exporters.nodeExporter.enabled | bool | `true` | Enable node-exporter. |
| exporters.nodeExporter.image.repository | string | `"quay.io/prometheus/node-exporter"` | Node-exporter image repository. |
| exporters.nodeExporter.image.tag | string | `"master"` | Node-exporter image tag. |
| exporters.nodeExporter.image.digest | string | `"sha256:5056097aa05bbb4dc22039cce8f2afa04d2a5223170d8d9b27b409955e3eb19d"` | Node-exporter image digest. |
| exporters.nodeExporter.image.pullPolicy | string | `"IfNotPresent"` | Node-exporter image pull policy. |
| exporters.nodeExporter.resources | object | `{}` | Node-exporter resource requests/limits. |
| exporters.nodeExporter.nodeSelector | object | `{}` | Node-exporter node selector. |
| exporters.nodeExporter.affinity | object | `{}` | Node-exporter affinity rules. |
| exporters.nodeExporter.tolerations | list | `[]` | Node-exporter tolerations. |
| exporters.kubeStateMetrics.enabled | bool | `true` | Enable kube-state-metrics. |
| exporters.kubeStateMetrics.image.repository | string | `"registry.k8s.io/kube-state-metrics/kube-state-metrics"` | Kube-state-metrics image repository. |
| exporters.kubeStateMetrics.image.tag | string | `"v2.18.0"` | Kube-state-metrics image tag. |
| exporters.kubeStateMetrics.image.digest | string | `"sha256:1545919b72e3ae035454fc054131e8d0f14b42ef6fc5b2ad5c751cafa6b2130e"` | Kube-state-metrics image digest. |
| exporters.kubeStateMetrics.image.pullPolicy | string | `"IfNotPresent"` | Kube-state-metrics image pull policy. |
| exporters.kubeStateMetrics.resources | object | `{}` | Kube-state-metrics resource requests/limits. |
| exporters.kubeStateMetrics.nodeSelector | object | `{}` | Kube-state-metrics node selector. |
| exporters.kubeStateMetrics.affinity | object | `{}` | Kube-state-metrics affinity rules. |
| exporters.kubeStateMetrics.tolerations | list | `[]` | Kube-state-metrics tolerations. |
| grafana.enabled | bool | `true` | Enable Grafana. |
| grafana.image.repository | string | `"grafana/grafana"` | Grafana image repository. |
| grafana.image.tag | string | `"12.3.1"` | Grafana image tag. |
| grafana.image.digest | string | `"sha256:2175aaa91c96733d86d31cf270d5310b278654b03f5718c59de12a865380a31f"` | Grafana image digest. |
| grafana.image.pullPolicy | string | `"IfNotPresent"` | Grafana image pull policy. |
| grafana.resources | object | `{}` | Grafana resource requests/limits. |
| grafana.nodeSelector | object | `{}` | Grafana node selector. |
| grafana.affinity | object | `{}` | Grafana affinity rules. |
| grafana.tolerations | list | `[]` | Grafana tolerations. |
| datasources.enabled | bool | `true` | Enable datasource provisioning. |
| datasources.prometheus.enabled | bool | `true` | Enable Prometheus datasource and deployment. |
| datasources.prometheus.image.repository | string | `"prom/prometheus"` | Prometheus image repository. |
| datasources.prometheus.image.tag | string | `"v3.5.1"` | Prometheus image tag. |
| datasources.prometheus.image.digest | string | `"sha256:38c3b05c3bc744ff1b0b7b4eb82196026442845e62a1e2073795565da506d7a2"` | Prometheus image digest. |
| datasources.prometheus.image.pullPolicy | string | `"IfNotPresent"` | Prometheus image pull policy. |
| datasources.prometheus.resources | object | `{}` | Prometheus resource requests/limits. |
| datasources.prometheus.nodeSelector | object | `{}` | Prometheus node selector. |
| datasources.prometheus.affinity | object | `{}` | Prometheus affinity rules. |
| datasources.prometheus.tolerations | list | `[]` | Prometheus tolerations. |
| datasources.loki.enabled | bool | `true` | Enable Loki datasource and deployment. |
| datasources.loki.image.repository | string | `"grafana/loki"` | Loki image repository. |
| datasources.loki.image.tag | string | `"3.6.4"` | Loki image tag. |
| datasources.loki.image.digest | string | `"sha256:be31579ac047e9f78b81e48f3b69d3af709e7299b431d5aa78bcda43382f9511"` | Loki image digest. |
| datasources.loki.image.pullPolicy | string | `"IfNotPresent"` | Loki image pull policy. |
| datasources.loki.url | string | `""` | Override Loki service URL for the Grafana datasource. |
| datasources.loki.resources | object | `{}` | Loki resource requests/limits. |
| datasources.loki.nodeSelector | object | `{}` | Loki node selector. |
| datasources.loki.affinity | object | `{}` | Loki affinity rules. |
| datasources.loki.tolerations | list | `[]` | Loki tolerations. |
| logs.promtail.enabled | bool | `true` | Enable Promtail log collection. |
| logs.promtail.image.repository | string | `"grafana/promtail"` | Promtail image repository. |
| logs.promtail.image.tag | string | `"3.6.4"` | Promtail image tag. |
| logs.promtail.image.digest | string | `"sha256:ba2f727003e28541bf8a9b9ff92a8adbe5a48f1cfe388493f9266bbd94891fe3"` | Promtail image digest. |
| logs.promtail.image.pullPolicy | string | `"IfNotPresent"` | Promtail image pull policy. |
| logs.promtail.clientUrl | string | `""` | Override Loki client URL. |
| logs.promtail.resources | object | `{}` | Promtail resource requests/limits. |
| logs.promtail.nodeSelector | object | `{}` | Promtail node selector. |
| logs.promtail.affinity | object | `{}` | Promtail affinity rules. |
| logs.promtail.tolerations | list | `[]` | Promtail tolerations. |
| dashboards.enabled | bool | `true` | Enable dashboard provisioning. |

## Troubleshooting
- Grafana CrashLoop on dashboard provisioning: `kubectl logs deploy/continuous-monitoring-continuous-monitoring-grafana` and confirm dashboard provider configmaps exist.
- Dashboards missing: ensure `dashboards.enabled=true` and the corresponding exporter is enabled; check with `kubectl get configmaps | rg dashboard`.
- Helm stuck in pending upgrade: `helm history continuous-monitoring` then `helm rollback continuous-monitoring <rev>`.
- Cluster unreachable: `kubectl cluster-info` and confirm your kubeconfig context.
