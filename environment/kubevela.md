# KubeVela install

## KubeVela

KubeVela is a modern software delivery and management control plane. For complete information on the installation of kubevela you can visit their [official website](https://kubevela.io/).

## Requirements

KubeVela relies on Kubernetes as a control plane. The control plane could be any managed Kubernetes offering or your own clusters.

- Kubernetes cluster >= v1.19 && <= v1.24

*Note: For the deployment of the [oam-applications project](https://github.com/activa-prefapp/oam-applications), kubevela has been installed on AWS EKS Service.*


## Install KubeVela on Kubernetes


You can install KubeVela by using the [CLI](https://kubevela.io/docs/installation/kubernetes#install-vela-cli):

```
$ vela install
```


If you are helm user, you can also use helm to install KubeVela core:

```
helm repo add kubevela https://kubevela.github.io/charts/
helm repo update
helm install --create-namespace -n vela-system kubevela kubevela/vela-core --wait
```

You can use the helm_release resource type in Terraform for the installation of KubeVela.

```
resource "helm_release" "kubevela" {
  name       = "kubevela"
  repository = "https://kubevela.github.io/charts/"
  chart      = "vela-core"
  version    = var.kubevela_version
  namespace  = "vela-system"
}
```

You can verify the installation with the following command

```
vela system info
```

## Advanced Installation

The KubeVela installation in Kubernetes is configured with a list of predefined parameters. You can consult the list in the official documentation in the [Advanced Installation section](https://kubevela.net/docs/platform-engineers/system-operation/bootstrap-parameters).

### 1. Consolidation

One of the predefined parameters has to do with the elapsed time for change checking to verify the consolidation between the OAM entities and the generated objects. 

This parameter is application-re-sync-period and is preset to 5 min. You can set the value of this parameter at the time of deploying vela-core

For example, if you use terraform and helm for deployment you can set the value for consolidation revision as follows:

```
resource "helm_release" "kubevela" {
  name       = "kubevela"
  repository = "https://kubevela.github.io/charts/"
  chart      = "vela-core"
  version    = var.kubevela_version
  namespace  = "vela-system"

  set {
    name  = "controllerArgs.reSyncPeriod"
    value = "1m"
  }

}
```


## References

- [KubeVela official website](https://kubevela.io/)

- [KubeVela docs](https://kubevela.io/docs/installation/kubernetes)

*[Back to contents](../README.md)*