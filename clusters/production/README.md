# Production Cluster

## Longhorn Requirements

```shell
longhornctl check preflight --kube-config=/path/to/kubeconfig
longhornctl install preflight --kube-config=/path/to/kubeconfig
# Verify installation
longhornctl check preflight --kube-config=/path/to/kubeconfig
```

## Discord Alerting

### Provider

```yaml
apiVersion: notification.toolkit.fluxcd.io/v1beta3
kind: Provider
metadata:
  name: discord
  namespace: flux-system
spec:
  type: discord
  secretRef:
    name: discord-webhook
```

### Alert

```yaml
apiVersion: notification.toolkit.fluxcd.io/v1beta3
kind: Alert
metadata:
  name: discord
  namespace: flux-system
spec:
  summary: "alerting"
  eventMetadata:
    env: "production"
    cluster: "my-cluster"
  providerRef:
    name: discord
  eventSeverity: info
  eventSources:
    - kind: GitRepository
      name: '*'
    - kind: Kustomization
      name: '*'
    - kind: HelmRepository
      name: '*'
    - kind: HelmChart
      name: '*'
    - kind: HelmRelease
      name: '*'
```
