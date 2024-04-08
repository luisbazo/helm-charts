[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/openshift-bootstraps)](https://artifacthub.io/packages/search?repo=openshift-bootstraps)
![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)

# Install IBM License Operator

This simply installs IBM License Operator
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

```
