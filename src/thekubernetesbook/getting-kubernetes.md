### 3: Getting Kubernetes

• Docker Desktop
• [K3d](https://k3d.io)
• [KinD](https://kind.sigs.k8s.io/)

> It’s important that your kubectl version is no more than one minor version higher or
lower than your cluster. For example, if your cluster is running Kubernetes 1.26.x, your
kubectl should be no lower than 1.25.x and no higher than 1.27.x.

> The kubectl configuration file is called config and lives in a hidden directory called
.kube in your home directory ($HOME/.kube/config). We normally call it the “kubecon-
fig” file and it contains definitions for:
• Clusters
• Users (credentials)
• Contexts

```bash
$ kubectl config current-context

$ kubectl config use-context docker-desktop
$ kubectl config current-context
```