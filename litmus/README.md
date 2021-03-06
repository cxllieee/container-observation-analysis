# Chaos Engineering 
Choas experiments will be conducting through litmus. [chaos hub](https://hub.litmuschaos.io/) provides a variety of chaos experiments which are readily available for use. Sourcing through the different experiments u can then apply it to your kubernetes cluster with the ability to tweak the experiment specifications to your needs. This will start a chaos runner and an experiment job which will act on your cluster and finally you can review the chaos experiment result to see if the cluster has passed the test.

This picture shows the flow of a chaos experiment:

![litmus](../images/litmus-flow.png)

## Install Litmus
Apply the LitmusChaos Operator manifest:

```console
kubectl apply -f https://litmuschaos.github.io/litmus/litmus-operator-v1.13.8.yaml
```

## Install Chaos Experiments
Chaos experiments contain the actual chaos details. These experiments are installed on your cluster as Kubernetes CRs. The Chaos Experiments are grouped as Chaos Charts and are published on [chaos hub](https://hub.litmuschaos.io/).

```console
kubectl apply -f https://hub.litmuschaos.io/api/chaos/1.13.8?file=charts/generic/experiments.yaml -n application
```

## Conduct an experiment

### Annotate application
Annotate deployments which the chaos experiments will act on
```console
kubectl annotate deploy/yelb-appserver litmuschaos.io/chaos="true" -n application
```

### Label application
Add labels to deployments so chaos engine can detect the deployment
```console
kubectl edit deploy/yelb-appserver -n application
```

Add the following lines under `metadata`
```
labels: 
  app: yelb-appserver
```

### Setup Service account
A service account should be created to allow chaosengine to run experiments in your application namespace. Apply the specific experiement rbac.yaml file. 
This service account has just enough permissions needed to run the pod-delete chaos experiment.

### Prepare ChaosEngine
ChaosEngine connects the application instance to a Chaos Experiment. Apply the specific experiement chaosengine.yaml file. 

### Observe Chaos results
Describe the ChaosResult to know the status of each experiment. The status.verdict is set to `Awaited` when the experiment is in progress, eventually changing to either `Pass` or `Fail`.

```console
kubectl get chaosresults -n application
kubectl describe chaosresults -n application
```
