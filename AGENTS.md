# Repository Guidelines

## Project Structure & Module Organization
- `charts/continuous-monitoring/` contains the Helm chart source (Chart.yaml, values, templates).
- `charts/continuous-monitoring/templates/` holds Kubernetes manifests rendered by Helm.
- `charts/continuous-monitoring/files/` is for extra static files consumed by templates.
- `artifacthub-repo.yml` stores Artifact Hub metadata for the Helm repo.

## Build, Test, and Development Commands
- `helm lint charts/continuous-monitoring`: validate chart structure and template syntax.
- `helm template continuous-monitoring charts/continuous-monitoring`: render manifests locally for review.
- `helm package charts/continuous-monitoring`: create a chart archive for release.
- `helm repo index .`: update `index.yaml` for the hosted chart repository.

## Coding Style & Naming Conventions
- Use 2-space indentation in YAML files; keep values in `values.yaml` ordered logically by feature area.
- Follow Helm chart conventions: lowercase chart name, template filenames in kebab-case (for example, `service-account.yaml`).
- Keep template logic minimal; prefer values-driven configuration to hardcoded defaults.

## Testing Guidelines
- No automated test suite is present. Use `helm lint` and `helm template` output as the primary validation.
- When changing values, update the `charts/continuous-monitoring/README.md` values table accordingly.

## Commit & Pull Request Guidelines
- Recent history mixes Conventional Commits (`feat: ...`) with sentence-case messages. Prefer Conventional Commits when possible for clarity.
- Keep commits focused (one change theme per commit).
- PRs should describe the chart changes, include rendered examples if templates change, and note any value additions or breaking changes.

## Release & Configuration Notes
- The published Helm repo URL is `https://firasmosbehi.github.io/continuous-monitoring-chart`.
- Update `artifacthub-repo.yml` if repository metadata changes.
