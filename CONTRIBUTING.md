# Contributing

Thanks for contributing to the Continuous Monitoring Helm chart. This project focuses on a clean, maintainable chart that is easy to install and upgrade.

## What to work on
- Chart improvements (templates, values, or defaults).
- Documentation updates (`README.md`, `charts/continuous-monitoring/README.md`, `AGENTS.md`).
- Bug fixes related to Helm rendering or Kubernetes compatibility.

## Development workflow
1) Create a topic branch from `main`.
2) Make focused changes (one feature or fix per PR).
3) Validate locally:
   - `helm lint charts/continuous-monitoring`
   - `helm template continuous-monitoring charts/continuous-monitoring`
4) Update docs if values or behavior change.

## Chart conventions
- YAML uses 2-space indentation.
- Keep Helm logic minimal; prefer values-driven configuration.
- Template names are kebab-case (for example, `service-account.yaml`).
- Dashboards are provisioned only when their exporter is enabled.

## Values and documentation
- If you add or rename values, update `charts/continuous-monitoring/README.md`.
- Keep the values table accurate and add short examples for new flags.

## Commit and PR guidance
- Prefer Conventional Commits (`feat:`, `fix:`, `docs:`).
- PRs should explain the change, note new values, and include rendered YAML if templates change.

## Release notes (maintainers)
- Bump chart version in `charts/continuous-monitoring/Chart.yaml`.
- Package and update the repository index:
  - `helm package charts/continuous-monitoring`
  - `helm repo index .`
