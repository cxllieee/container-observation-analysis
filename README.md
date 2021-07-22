# container-observation-analysis
This project is intended to test an application's resiliency through various chaos experiments. The chosen testing application is taken from
https://github.com/mreferre/yelb. You can refer to the original documentation for understanding of Yelb's functions and architecture.

The picture below shows the architecture of Yelb:

![yelb-architecture](images/yelb-architecture.png)

## Project Setup

### Create the kubernetes cluster in minikube
Using minikube, create the cluster with 3 nodes and with sufficient memory.

```console
minikube start --driver=virtualbox --no-vtx-check --nodes 3 -p test --memory 3086
```

### Enable Addons
Enable the respective addons to the minikube cluster.

```console
minikube -p test addons enable ingress
minikube -p test addons enable metrics-server
```

### Deploy yelb 
Deploy Yelb to the cluster in the `application` namespace

```console
kubectl create ns application
kubectl apply -f /Kubernetes/yaml -n application
```

## Helm Charts
[Helm](https://helm.sh) must be installed to use the charts.
Please refer to Helm's [documentation](https://helm.sh/docs/) to get started.

Once Helm is set up properly, add the repo as follows:
```console
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

### Install Chart
Install the respective charts with values under `helm-values`.

```console
helm install [RELEASE_NAME] prometheus-community/prometheus
helm install [RELEASE_NAME] prometheus-community/prometheus-postgres-exporter
helm install [RELEASE_NAME] prometheus-community/prometheus-redis-exporter
```
## Prometheus
Once prometheus is install add the custom rules from `alerts.yaml` into  .
