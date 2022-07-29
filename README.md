# Secrets-store-csi-driver-provider-local


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

## Usage

### Create your own SecretProviderClass Object

To use the Secrets Store CSI driver, create a `SecretProviderClass` custom resource to provide driver configurations and provider-specific parameters to the CSI driver.

A `SecretProviderClass` custom resource should have the following components:

```yaml
apiVersion: secrets-store.csi.x-k8s.io/v1
kind: SecretProviderClass
metadata:
  name: my-provider
spec:
  provider: csilocal
  parameters:                                 # provider-specific parameters
```

### Update your Deployment Yaml

To ensure your application is using the Secrets Store CSI driver, update your deployment yaml to use the `secrets-store.csi.k8s.io` driver and reference the `SecretProviderClass` resource created in the previous step.

```yaml
volumes:
  - name: secrets-store-inline
    csi:
      driver: secrets-store.csi.k8s.io
      readOnly: true
      volumeAttributes:
        secretProviderClass: "my-provider"
```
### Secret Content is Mounted on Pod Start

On pod start and restart, the driver will communicate with the provider using gRPC to retrieve the secret content from the external Secrets Store you have specified in the `SecretProviderClass` custom resource. Then the volume is mounted in the pod as `tmpfs` and the secret contents are written to the volume.

To validate, once the pod is started, you should see the new mounted content at the volume path specified in your deployment yaml.

```bash
kubectl exec secrets-store-inline -- ls /mnt/secrets-store/
foo
```

## TODO

- [ ] UI
- [ ] from JSON
- [ ] from memory
- [ ] from configmap
- [ ] sync secrets to kubernetes
- [ ] rotation
- [ ] metrics
