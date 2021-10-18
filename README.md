# Verdaccio Charts for GKE

[Verdaccio](https://www.verdaccio.org) is a lightweight private
[NPM](https://www.npmjs.com) proxy registry.

## TL;DR;

```bash
helm repo add verdaccio-gke-charts https://xlts-dev.github.io/verdaccio-gke-charts
helm repo update
helm install verdaccio-gke-charts/verdaccio-gke-charts
```

## Introduction

This chart bootstraps a [Verdaccio](https://github.com/verdaccio/verdaccio)
deployment on a [Kubernetes](https://kubernetes.io) cluster using the
[Helm](https://helm.sh) package manager on [GKE](https://cloud.google.com/kubernetes-engine).

## Prerequisites

1. Create a [Google CLoud Project](https://console.cloud.google.com/)
1. Install the [gcloud SDK and CLI](https://cloud.google.com/sdk/docs/install) or use the
   [Cloud Shell](https://cloud.google.com/shell)
1. If using the CLI outside of Cloud Shell, run `gcloud config set project your-project`
1. [Create a GKE cluster](https://cloud.google.com/kubernetes-engine/docs/how-to/creating-an-autopilot-cluster)
1. Pick the cluster to work with using a command like
    - `gcloud container clusters get-credentials verdaccio-autopilot-cluster --region=us-central1`
1. Create a namespace (recommended)
    - `kubectl create namespace registry`
1. If not in the Cloud Shell, [Install Helm](https://helm.sh/docs/intro/install/) v3+

## Installing the Chart

### Add repository

```bash
helm repo add verdaccio-gke-charts https://xlts-dev.github.io/verdaccio-gke-charts
```

### Install Verdaccio chart

In this example we use `npm` as release name:

```bash
# Helm v3+
helm install npm --namespace registry verdaccio-gke-charts/verdaccio-gke-charts
```

### Deploy a specific Verdaccio version

```bash
helm install npm --set image.tag=4.6.2 verdaccio-gke-charts/verdaccio-gke-charts
```

### Upgrading Verdaccio

```bash
helm upgrade npm --namespace registry verdaccio-gke-charts/verdaccio-gke-charts
```

The command deploys Verdaccio on the GKE cluster in the default
configuration. The [configuration](#configuration) section lists the parameters
that can be configured during installation.

> **Tip**: List all releases using `helm list`.

## Uninstalling the Chart

To uninstall/delete the `npm` deployment:

```bash
helm uninstall npm --namespace registry
```

The command removes all the GKE and GCE components associated with the chart and
deletes the release.

## Configuration

The following table lists the configurable parameters of the Verdaccio chart,
and their default values.

| Parameter                          | Description                                                     | Default                        |
| ---------------------------------- | --------------------------------------------------------------- | ------------------------------ |
| `affinity`                         | Affinity for pod assignment                                     | `{}`                           |
| `existingConfigMap`                | Name of custom ConfigMap to use                                 | `false`                        |
| `image.pullPolicy`                 | Image pull policy                                               | `IfNotPresent`                 |
| `image.pullSecrets`                | Image pull secrets                                              | `[]`                           |
| `image.repository`                 | Verdaccio container image repository                            | `verdaccio/verdaccio`          |
| `image.tag`                        | Verdaccio container image tag                                   | `5.1.6`                        |
| `nodeSelector`                     | Node labels for pod assignment                                  | `{}`                           |
| `tolerations`                      | List of node taints to tolerate                                 | `[]`                           |
| `persistence.accessMode`           | PVC Access Mode for Verdaccio volume                            | `ReadWriteOnce`                |
| `persistence.enabled`              | Enable persistence using PVC                                    | `true`                         |
| `persistence.existingClaim`        | Use existing PVC                                                | `nil`                          |
| `persistence.mounts`               | Additional mounts                                               | `nil`                          |
| `persistence.size`                 | PVC Storage Request for Verdaccio volume                        | `8Gi`                          |
| `persistence.storageClass`         | PVC Storage Class for Verdaccio volume                          | `nil`                          |
| `persistence.selector`             | Selector to match an existing Persistent Volume                 | `{}` (evaluated as a template) |
| `persistence.volumes`              | Additional volumes                                              | `nil`                          |
| `podLabels`                        | Additional pod labels                                           | `{}` (evaluated as a template) |
| `podAnnotations`                   | Annotations to add to each pod                                  | `{}`                           |
| `priorityClass.enabled`            | Enable specifying pod priorityClassName                         | `false`                        |
| `priorityClass.name`               | PriorityClassName to be specified in pod spec                   | `""`                           |
| `replicaCount`                     | Desired number of pods                                          | `1`                            |
| `resources`                        | CPU/Memory resource requests/limits                             | `{}`                           |
| `service.annotations`              | Annotations to add to service                                   | none                           |
| `service.clusterIP`                | IP address to assign to service                                 | `""`                           |
| `service.externalIPs`              | Service external IP addresses                                   | `[]`                           |
| `service.loadBalancerIP`           | IP address to assign to load balancer (if supported)            | `""`                           |
| `service.loadBalancerSourceRanges` | List of IP CIDRs allowed access to load balancer (if supported) | `[]`                           |
| `service.port`                     | Service port to expose                                          | `80`                           |
| `service.targetPort`               | Container port to target                                        | `4873`                         |
| `service.type`                     | Type of service to create                                       | `ClusterIP`                    |
| `serviceAccount.create`            | Create service account                                          | `false`                        |
| `serviceAccount.name`              | Service account Name                                            | none                           |
| `extraEnvVars`                     | Define environment variables to be passed to the container      | `{}`                           |
| `extraInitContainers`              | Define additional initContainers to be added to the deployment  | `[]`                           |
| `securityContext`                  | Define Container Security Context                               | `{runAsUser=10001}`            |
| `podSecurityContext`               | Define Pod Security Context                                     | `{fsGroup=101}`                |
| `nameOverride`                     | Set resource name override                                      | `""`                           |
| `fullnameOverride`                 | Set resource fullname override                                  | `""`                           |
| `ingress.enabled`                  | Enable/Disable Ingress                                          | `false`                        |
| `ingress.className`                | Ingress Class Name (k8s `>=1.18` required)                      | `""`                           |
| `ingress.annotations`              | Ingress Annotations                                             | `{}`                           |
| `ingress.hosts`                    | List of Ingress Hosts                                           | `[]`                           |
| `ingress.paths`                    | List of Ingress Paths                                           | `["/"]`                        |
| `ingress.extraPaths`               | List of extra Ingress Paths                                     | `[]`                           |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```bash
helm install my-release --set service.type=LoadBalancer verdaccio-gke-charts/verdaccio-gke-charts
```

The above command sets the service type `LoadBalancer`.

Alternatively, a YAML file that specifies the values for the above parameters
can be provided while installing the chart. For example,

```bash
helm install my-release -f values.yaml verdaccio-gke-charts/verdaccio-gke-charts
```

> **Tip**: You can use the default [values.yaml](charts/verdaccio/values.yaml) as a starting point.

### Custom ConfigMap

When creating a new chart with this chart as a dependency, CustomConfigMap can
be used to override the default config.yaml provided. It also allows for
providing additional configuration files that will be copied into
`/verdaccio/conf`. In the parent chart's values.yaml, set the value to true and
provide the file `templates/config.yaml` for your use case.

### Persistence

The Verdaccio image stores persistence under `/verdaccio/storage` path of the
container. A dynamically managed Persistent Volume Claim is used to keep the
data across deployments, by default. This is known to work in GCE, AWS, and
minikube.
Alternatively, a previously configured Persistent Volume Claim can be used.

It is possible to mount several volumes using `Persistence.volumes` and
`Persistence.mounts` parameters.

#### Existing PersistentVolumeClaim

1. Create the PersistentVolume
1. Create the PersistentVolumeClaim
1. Install the chart

```bash
helm install npm \
    --set persistence.existingClaim=PVC_NAME \
    verdaccio-gke-charts/verdaccio-gke-charts
```
