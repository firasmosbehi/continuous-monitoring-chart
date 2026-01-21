# Future Features

This file tracks planned improvements for the Continuous Monitoring Helm chart.

## Planned
- Add optional persistence for Grafana, Prometheus, and Loki.
- Add configurable Grafana admin credentials via Kubernetes Secrets.
- Support configurable Grafana ingress (host, TLS, annotations).
- Add resource requests/limits defaults and guidance for sizing.
- Add Prometheus retention and scrape interval settings.
- Provide optional ServiceMonitors for Prometheus Operator setups.
- Add optional Alertmanager deployment and configuration.
- Add namespace-scoped RBAC option for kube-state-metrics.

## Metrics exporters
- Add database exporters (PostgreSQL, MySQL, MongoDB, Redis, Elasticsearch) with optional dashboards.
- Add application exporters (JVM/JMX, Node.js, Go, Python, NGINX, Apache, Kafka) with optional dashboards.
- Add Kubernetes control-plane exporters (etcd, kube-apiserver, kube-controller-manager, kube-scheduler) where accessible.
- Add cloud-native exporters (AWS CloudWatch/Exporter, kubelet/cAdvisor metrics toggles).

## Logs and traces
- Add Loki storage backends (S3, GCS) configuration.
- Add cluster log collection (promtail or fluent-bit) with configurable inputs and labels.
- Add traces collection with Tempo and OpenTelemetry Collector (OTLP gRPC/HTTP).

## Dashboards and integrations
- Add curated dashboards per exporter with versioned compatibility notes.
- Add optional Grafana sidecar support for external dashboard/config sources.
- Add integrations for common stacks (Prometheus Operator, kube-prometheus-stack).

## Nice to have
- Add dashboard provisioning for additional exporters.
- Include example values files for common environments (dev, staging, prod).
- Add CI linting for Helm chart and YAML schema checks.
