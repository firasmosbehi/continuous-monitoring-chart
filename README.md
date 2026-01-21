# Continuous Monitoring Helm Repository

This branch backs the GitHub Pages site for the Continuous Monitoring Helm chart. It hosts the chart package(s) and `index.yaml` used by Helm clients.

## Chart repository URL
`https://firasmosbehi.github.io/continuous-monitoring-chart`

## Quickstart
```sh
helm repo add continuous-monitoring https://firasmosbehi.github.io/continuous-monitoring-chart
helm install continuous-monitoring continuous-monitoring/continuous-monitoring
```

## Contents
- `index.yaml`: Helm repository index.
- `*.tgz`: Packaged chart releases.
- `index.html`: Human-friendly landing page.
