### 4: Working with Pods

```bash
$ kubectl explain pods --recursive
$ kubectl explain pod.spec.restartPolicy
```

1. Labels let you group Pods and associate them with other objects in powerful ways.
1. Annotations let you add experimental features and integrations with 3rd-party tools and services.
1. Probes let you test the health and status of Pods and the apps they run. This enables advanced scheduling, updates, and more.
1. Affinity and anti-affinity rules give you control over where in a cluster Pods are allowed to run.
1. Termination control lets you gracefully terminate Pods and the applications they run.
1. Security policies let you enforce security features.
1. Resource requests and limits let you specify minimum and maximum values for things like CPU, memory, and disk IO.

> Options include Always, OnFailure, and Never. Always is the default restart policy and appropriate for most long-lived Pods.
Appropriate container restart policies for short-lived Pods will usually be Never or OnFailure.

> Deployments, StatefulSets, and DaemonSets are examples of controllers designed for long-lived Pods.

> Jobs and CronJobs are examples designed for short-lived Pods.


#### Multi-container Pods

• Sidecar pattern
• Adapter pattern
• Ambassador pattern
• Init pattern


#### Hands-on with Pods

Straight away you can see four top-level resources:

• kind
• apiVersion
• metadata
• spec


#### Deploying Pods from a manifest file

```bash
$ kubectl apply -f pod.yml
$ kubectl get pods
$ kubectl get pods --watch
```

Introspecting running Pods

```bash
$ kubectl get pods hello-pod -o yaml
$ kubectl describe pods hello-pod

$ kubectl logs <pod>
$ kubectl logs multipod --container syncer

$ kubectl exec <pod-name> -- <command>
$ kubectl exec hello-pod -- ps
$ kubectl exec -it hello-pod -- sh
$ kubectl exec -it hello-pod --container syncer -- sh

$ kubectl edit pod hello-pod

$ kubectl delete pod git-sync
```
