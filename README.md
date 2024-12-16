# Heimdall

This repository is made for manage heimdall cluster with the help fluxcd, all manifests and charts are set up in a gitops way. So I can reuse it if a new cluster is set up.

## Context

The cluster is set up with [K0S](https://k0sproject.io/),
the management of deployments is made by [FluxCD](https://fluxcd.io/),
the management of storage is made by [Longhorn](https://longhorn.io/),
the management of secrets is made by [Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets).
