# SSSRYR (Painless Clusters Repository)

SSSRYR is a clusters repository, where template and application manifests of my collection are stored.

## 🔩 Kubernetes Framework

I used [K3s](https://docs.k3s.io/) for deploying the server node and his agent nodes.
Quite easy to deploy, clear documentations and also simple to add agent node to cluster.

### Default components

K3s deploys [Traefik](https://traefik.io/) as Ingress Controller for network routing.
[CoreDNS](https://coredns.io/) as DNS server for service discovery, flexibility and simplicity.
[Metrics Server](https://github.com/kubernetes-sigs/metrics-server) as container performance observer to manage provided resources.

### Deployment

First, deploy the server node that will manage agent node.(the server node is himself an agent node with control panel.)

> Warning, as requirements few ports should be open on the node, for agent and server joinings.

```bash
curl -sfL https://get.k3s.io | sh -
```

Then, deploy the agent nodes that will be manage by the server node.

```bash
curl -sfL https://get.k3s.io | K3S_URL=https://myserver:6443 K3S_TOKEN=mynodetoken sh -
```

> K3S_URL will be the public ip address of your server where is deployed the server node.

> K3S_TOKEN will be found at /var/lib/rancher/k3s/server/node-token on the server node.

### Prerequisites

Verify the availability of those nodes to **longhorn** with the help of [longhornctl](https://longhorn.io/docs/1.7.2/advanced-resources/longhornctl/).

> Preflight check of nodes.

```shell
longhornctl check preflight --kube-config=/path/to/kubeconfig
```

> Preflight install of nodes.

```shell
longhornctl install preflight --kube-config=/path/to/kubeconfig
```

## Infrastructure Frameworks

### 🔌 FluxCD

[FluxCD](https://fluxcd.io/) is a gitops tools that allows the kubernetes cluster to poll git version of a big deployment manifest and stay up to date.

#### **Deployment**

```bash
export GITHUB_TOKEN=<gh-token>
```

```bash
flux bootstrap github --token-auth --owner=noynto --repository=heimdall --branch=main --personal
```

### 🔐 Sealed Secrets

[Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets) is a gitops tool that help store sealed secrets on the repository even if this one is public. And those secrets are unsealed by the cluster.

#### **Deployment**

Thanks to FluxCD, the [manifests](infrastructure/sealed-secrets.yaml) are self-deployed, it's a helm chart.

### 📦 Longhorn

[Longhorn](https://longhorn.io/) is a volume/storage manager, it replicates data between nodes, helps to avoid lost of data with snapshots and backups.

#### **Deployment**

Thanks to FluxCD, the [manifests](infrastructure/longhorn.yaml) are self-deployed, it's a helm chart.

### 🔌 MetalLB

[MetalLB](https://metallb.io/) is a bare-metal cluster network load balancer for kubernetes.

#### **Deployment**

Thanks to FluxCD, the [manifests](infrastructure/metallb.yaml) are self-deployed, it's a helm chart.
