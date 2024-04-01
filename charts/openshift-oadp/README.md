[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/openshift-bootstraps)](https://artifacthub.io/packages/search?repo=openshift-bootstraps)
![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)

# Install Operator OADP

This simply installs OpenShift OADP Operator and validates the status of the installation.
It uses the Subchart:

* [helper-operator](https://github.com/tjungbauer/helm-charts/tree/main/charts/helper-operator): to create the required Operator resources
* [helper-status-checker](https://github.com/tjungbauer/helm-charts/tree/main/charts/helper-operator): to verify if the Deployments of this Operator are running.

It is best used with a GitOps approach such as Argo CD does. For example, https://github.com/tjungbauer/openshift-clusterconfig-gitops

## TL;DR

```console
helm repo add --force-update tjungbauer https://charts.stderr.at
helm repo update
```

## Prerequisites

* Kubernetes 1.12+
* Helm 3

## Installing the Chart

To install the chart with the release name `my-release`:

```console
helm install my-release luisbazo/openshift-oadp
```

The command deploys the chart on the Kubernetes cluster in the default configuration.

## Uninstalling the Chart

To uninstall/delete the my-release deployment:

```console
helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters
The following table lists the configurable parameters of the chart and their default values. Only variables of this specific Helm Chart are listed. For the values of the Subchart read the appropriate README of the Subcharts.

| Parameter                                 | Description                                   | Default                                                 |
|-------------------------------------------|-----------------------------------------------|---------------------------------------------------------|
| `loggingConfig.enabled` | Configure Cluster Logging | `` |


## Example

```yaml
---

# Install Operator Compliance Operator
# Deploys Operator --> Subscription and Operatorgroup
# Syncwave: 0
---
helper-operator:
  operators:
    redhat-oadp-operator:
      enabled: false
      syncwave: '0'
      namespace:
        name: openshift-adp
        create: true
      subscription:
        channel: stable-1.13
        approval: Automatic
        operatorName: redhat-oadp-operator
        source: redhat-operators
        sourceNamespace: openshift-marketplace
      operatorgroup:
        create: true

helper-status-checker:
  enabled: false

  checks:

    - operatorName: redhat-oadp-operator
      namespace:
        name: openshift-adp
      syncwave: 3

      serviceAccount:
        name: "status-checker-oadp"


```
