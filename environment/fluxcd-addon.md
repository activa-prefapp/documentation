# FluxCD addon install

FluxCD is the necessary add-on for the deployment of Helm through KubeVela. 

## FluxCD

The Helm Chart is currently supported by the FluxCD addon. In addition to the Helm Chart, FluxCD addon also supports Kustomize. 

For complete information on installing kubevela or any of its add-ons, please visit the [official website](https://kubevela.io/).

## Requirements

- Kubernetes cluster >= v1.19 && <= v1.24
- KubeVela installed

## Enable FluxCD addon

The delivery of Helm Chart depends on the FluxCD addon in KubeVela, you need to enable the addon before you start. You can refer to the addon management document for more information about addons.

You can perform the installation with the following command

```
vela addon enable fluxcd
```

## References

- [KubeVela official website](https://kubevela.io/)

- [KubeVela docs](https://kubevela.io/docs/end-user/components/more)