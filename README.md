# Continuous Monitoring Helm Chart

This repository contains the Helm chart for the Continuous Monitoring service.

## Repository layout

- `charts/continuous-monitoring`: Helm chart source
- `artifacthub-repo.yml`: Artifact Hub metadata

## Local development

```sh
helm lint charts/continuous-monitoring
helm template continuous-monitoring charts/continuous-monitoring
```

## Publishing

- Package the chart: `helm package charts/continuous-monitoring`
- Update your Helm repo index: `helm repo index .`
- Upload packaged chart and `index.yaml` to your hosting (GitHub Pages, S3, etc.)
- Register the repo in Artifact Hub and keep `artifacthub-repo.yml` updated

## License

Apache-2.0
