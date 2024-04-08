# My Helm Chart Collection

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Lint and Test Charts](https://github.com/tjungbauer/helm-charts/actions/workflows/lint_and_test_charts.yml/badge.svg)](https://github.com/tjungbauer/helm-charts/actions/workflows/lint_and_test_charts.yml)
[![Release Charts](https://github.com/tjungbauer/helm-charts/actions/workflows/release.yml/badge.svg)](https://github.com/tjungbauer/helm-charts/actions/workflows/release.yml)

This Helm Chart repository contains Charts, which I use to deploy my Demos on OpenShift/Kubernetes.
It is mainly used by [Cluster Bootstrap](https://github.com/tjungbauer/openshift-clusterconfig-gitops) but it can be used individually as well.

## Examples for usage of some of the charts developed in GITOPS

GITOPS - OPERATOR deployment ODF

Inspiration and charts examples I have forked and customized

https://www.redhat.com/en/blog/operator-installation-with-argo-cd/gitops

This command in needed to provide ARGO CD with admin priviledges in the cluster. It can be changed by on a per namespace basis adding just admin priviledges to the namespace required

oc adm policy add-cluster-role-to-user cluster-admin system:serviceaccount:openshift-gitops:openshift-gitops-argocd-application-controller

##ARGO APPLICATION INFRA MACHINESET

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: in-cluster-install-machineset-infra
  namespace: openshift-gitops
spec:
  destination:
    namespace: default
    server: 'https://kubernetes.default.svc'
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
  info:
    - name: Description
      value: Machine set to infra nodes
  project: default
  source:
    path: charts/openshift-machineset
    repoURL: 'https://github.com/luisbazo/helm-charts'
    targetRevision: main
```


##OFD installation

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: in-cluster-install-openshift-data-foundation
  namespace: openshift-gitops
spec:
  destination:
    namespace: default
    server: 'https://kubernetes.default.svc'
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
  info:
    - name: Description
      value: Deploy Openshift Data foundation
  project: default
  source:
    helm:
      valueFiles:
        - values.yaml
    path: charts/openshift-data-foundation
    repoURL: 'https://github.com/luisbazo/helm-charts'
    targetRevision: main
```

##ARGO APPLICATION OADP OPERATOR

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: in-cluster-install-oadp-operator
  namespace: openshift-gitops
spec:
  destination:
    namespace: default
    server: 'https://kubernetes.default.svc'
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
  info:
    - name: Description
      value: Deploy OADP Operator
  project: default
  source:
    helm:
      valueFiles:
        - values.yaml
    path: charts/openshift-oadp
    repoURL: 'https://github.com/luisbazo/helm-charts'
    targetRevision: main
```


##ARGO APPLICATION IBM LICENSE SERVER

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: in-cluster-install-license-server
  namespace: openshift-gitops
spec:
  destination:
    namespace: default
    server: 'https://kubernetes.default.svc'
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
  info:
    - name: Description
      value: Deploy IBM License Server
  project: default
  source:
    helm:
      valueFiles:
        - values.yaml
    path: charts/ibm-license-operator
    repoURL: 'https://github.com/luisbazo/helm-charts'
    targetRevision: main
```

##OPENSHIFT LOGGING

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: in-cluster-install-openshift-logging
  namespace: openshift-gitops
spec:
  destination:
    namespace: default
    server: 'https://kubernetes.default.svc'
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
  info:
    - name: Description
      value: Deploy Openshift Logging
  project: default
  source:
    helm:
      valueFiles:
        - values.yaml
    path: charts/openshift-logging
    repoURL: 'https://github.com/luisbazo/helm-charts'
    targetRevision: main
```

##PROMETHEUS ALERTING

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: in-cluster-install-prometheus-sample-app
  namespace: openshift-gitops
spec:
  destination:
    namespace: default
    server: 'https://kubernetes.default.svc'
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
  info:
    - name: Description
      value: Deploy Openshift Logging
  project: default
  source:
    helm:
      valueFiles:
        - values.yaml
    path: charts/prometheus-example-app
    repoURL: 'https://github.com/luisbazo/helm-charts'
    targetRevision: main
```

If interested in developing new custom grafana dashboards please visit,

https://kamsjec.medium.com/custom-grafana-dashboards-for-red-hat-openshift-container-platform-4-x-9495678b714c

##Some PROMETHEUS EXAMPLE QUERIES to visualize the available to RESERVE node CPU.

This is important since new pods won't get scheduled in the nodes if no avaiblabe to reserve CPU is found in the cluster

(sum(kube_node_status_allocatable{cluster="", node=~"ocpinstall-l7rz9-master-0", resource="cpu"}) / sum(kube_node_status_capacity{cluster="", node=~"ocpinstall-l7rz9-master-0", resource="cpu"}))*100

sum(cluster:namespace:pod_cpu:active:kube_pod_container_resource_requests{cluster=""}) by (node) / sum(kube_node_status_capacity{cluster="", resource="cpu"}) by (node)

sum(cluster:namespace:pod_cpu:active:kube_pod_container_resource_requests{cluster=""}) by (node) / sum(kube_node_status_allocatable{cluster="", resource="cpu"}) by (node)
