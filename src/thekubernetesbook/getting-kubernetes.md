### 3: Getting Kubernetes

1. Docker Desktop
1. [K3d](https://k3d.io)
1. [KinD](https://kind.sigs.k8s.io/)

> It’s important that your kubectl version is no more than one minor version higher or lower than your cluster. For example, if your cluster is running Kubernetes 1.26.x, your kubectl should be no lower than 1.25.x and no higher than 1.27.x.

> The kubectl configuration file is called config and lives in a hidden directory called .kube in your home directory ($HOME/.kube/config). We normally call it the “kubeconfig” file and it contains definitions for:

1. Clusters
1. Users (credentials)
1. Contexts

```bash
$ kubectl config view

$ kubectl config current-context
$ kubectl config use-context docker-desktop
```
