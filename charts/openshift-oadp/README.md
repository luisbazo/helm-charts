[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/openshift-bootstraps)](https://artifacthub.io/packages/search?repo=openshift-bootstraps)
![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)

# Install Operator OADP

This simply installs OpenShift OADP Operator and validates the status of the installation.
It uses the Subchart:

* [helper-operator](https://github.com/tjungbauer/helm-charts/tree/main/charts/helper-operator): to create the required Operator resources
* [helper-status-checker](https://github.com/tjungbauer/helm-charts/tree/main/charts/helper-operator): to verify if the Deployments of this Operator are running.

It is chart is to be used with a GitOps approach such as Argo CD does. For example, https://github.com/tjungbauer/openshift-clusterconfig-gitops


## Prerequisites

* Kubernetes 1.12+
* Helm 3


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
