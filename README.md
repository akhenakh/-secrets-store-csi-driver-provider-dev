# Secrets-store-csi-driver-provider-dev


A developer provider for the [Secret Store CSI
Driver](https://github.com/kubernetes-sigs/secrets-store-csi-driver). Allows you
to access secrets stored in a virtual secret manager as files mounted in Kubernetes pods.
Use it to learn and explore the CSI secret interface, and test good practices for reading secrets from pods.

Can be used on dev clusters, NOT SAFE for production, NOT SAFE to protect your secrets.


## Install

Works on any Kubernetes flavors.

First you need to install the Secrets Store CSI Driver, [full documentation](https://secrets-store-csi-driver.sigs.k8s.io/getting-started/installation.html):

```
helm repo add secrets-store-csi-driver https://kubernetes-sigs.github.io/secrets-store-csi-driver/charts
helm install csi-secrets-store secrets-store-csi-driver/secrets-store-csi-driver --namespace kube-system
```

## TODO

- [ ] UI
- [ ] from JSON
- [ ] from memory
- [ ] from configmap
- [ ] sync secrets to kubernetes
- [ ] rotation
- [ ] metrics
