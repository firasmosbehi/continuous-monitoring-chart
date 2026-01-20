# continuous-monitoring

Helm chart for deploying the Continuous Monitoring service.

## Install

```sh
helm repo add continuous-monitoring https://your-org.github.io/continuous-monitoring
helm install continuous-monitoring continuous-monitoring/continuous-monitoring
```

## Values

| Key | Type | Default | Description |
| --- | ---- | ------- | ----------- |
| image.repository | string | `"ghcr.io/your-org/continuous-monitoring"` | Container image repository. |
| image.tag | string | `"0.1.0"` | Container image tag. |
| service.type | string | `"ClusterIP"` | Service type. |
| service.port | int | `80` | Service port. |
| containerPort | int | `8080` | Container port. |
| ingress.enabled | bool | `false` | Enable ingress. |
