# Helm Charts
[Helm](https://helm.sh) must be installed to use the charts.
Please refer to Helm's [documentation](https://helm.sh/docs/) to get started.

Once Helm is set up properly, add the repo as follows:
```console
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

## Install Chart
Install the respective charts with values under `helm-values` in the `monitoring` namespace.

```console
helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring -f .\prometheus.yaml
helm install postgres-exporter prometheus-community/prometheus-postgres-exporter -n monitoring -f .\postgres-values.yaml
helm install redis-exporter prometheus-community/prometheus-redis-exporter -n monitoring -f .\redis-values.yaml
```
## Prometheus
Once prometheus is install add the custom rules from `alerts.yaml` into  .
```console
kubectl edit prometheusrules prometheus-kube-prometheus-kubernetes-resources -n monitoring
```

## Alertmanager
Input your slack API and respective alert channel in the `prometheus` file

## Windows Exporter
Input local host ip address under prometheus spec
