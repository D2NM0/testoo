# Datadog Helm Charts

[![Artifact HUB](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/datadog)](https://artifacthub.io/packages/search?repo=datadog)

Official Helm charts for Datadog products. Currently supported:
- [Datadog Agents](charts/datadog/README.md) (`datadog/datadog`)
- [Datadog Operator](charts/datadog-operator/README.md) (`datadog/datadog-operator`)
- [Extended DaemonSet](charts/extended-daemon-set/README.md) (`datadog/extendeddaemonset`)
- [Observability Pipelines Worker](charts/observability-pipelines-worker/README.md) (`datadog/observability-pipelines-worker`)
- [Synthetics Private Location](charts/synthetics-private-location/README.md) (`datadog/synthetics-private-location`)

## Maintainers

- Slack support : https://adeo-tech-community.slack.com/archives/C01V0MNQ9PW
- E-mail : gtdobservabilitydomain@adeo.com

## How to use Datadog Helm repository

You need to add this repository to your Helm repositories:

```shell
helm repo add datadog https://helm.datadoghq.com
helm repo update
```


## Agent Version Update 

versions updates across environments (Dev → NPRD → Prod) are automatically managed.

### Workflow

1. **Dependabot PR for Dev**
   - Dependabot creates a pull request for the **Dev environment** with the new versions.

2. **Progressive Environment Sync**
   - When a version is updated in **Dev**, a workflow automatically creates a PR for **Nprd**.
   - When a version is updated in **Nprd**, a workflow automatically creates a PR for **Prod**.

3. **Automatic ServiceNow Change Creation**
   - Any version update in an environment triggers the creation of a **ServiceNow standard change**.
