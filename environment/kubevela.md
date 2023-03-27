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
helm repo add kubevela https://charts.kubevela.net/core
helm repo update
helm install --create-namespace -n vela-system kubevela kubevela/vela-core --wait
```

You can use the helm_release resource type in Terraform for the installation of KubeVela.

```
resource "helm_release" "kubevela" {
  name       = "kubevela"
  repository = "https://charts.kubevela.net/core"
  chart      = "vela-core"
  version    = var.kubevela_version
  namespace  = "vela-system"
}
```

You can verify the installation with the following command

```
vela system info
```

## References

- [KubeVela official website](https://kubevela.io/)

- [KubeVela docs](https://kubevela.io/docs/installation/kubernetes)