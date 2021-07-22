# Helm Charts
[Helm](https://helm.sh) must be installed to use the charts.
Please refer to Helm's [documentation](https://helm.sh/docs/) to get started.

Once Helm is set up properly, add the repo as follows:
```console
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

## Install Chart
Install the respective charts with values under `helm-values`.

```console
helm install [RELEASE_NAME] prometheus-community/prometheus
helm install [RELEASE_NAME] prometheus-community/prometheus-postgres-exporter
helm install [RELEASE_NAME] prometheus-community/prometheus-redis-exporter
```
## Prometheus
Once prometheus is install add the custom rules from `alerts.yaml` into  .

## Alertmanager
Input your slack API and respective alert channel in the `prometheus` file

## Windows Exporter
Input local host ip address under prometheus spec
