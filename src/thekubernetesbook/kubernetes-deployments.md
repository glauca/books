### 6: Kubernetes Deployments

deploy.yml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-deploy
spec:
  replicas: 10
  selector: # Pods
    matchLabels:
      app: hello-world
  revisionHistoryLimit: 5
  progressDeadlineSeconds: 300
  minReadySeconds: 10
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
      - name: hello-pod
        image: nigelpoulton/k8sbook:1.0
        ports:
        - containerPort: 8080
```

> revisionHistoryLimit tells Kubernetes to keep the configs of the previous 5 releases.
This means the previous 5 ReplicaSet objects will be kept and you can easily rollback to
any of them.

> progressDeadlineSeconds governs how long each new Pod replica has to start before
Kubernetes considers the rollout to have stalled. The config shown gives each Pod
replica its own 5-minute window to come up.

> .spec.minReadySeconds throttles the rate at which replicas are replaced. The one in the
example tells Kubernetes that any new replica must be up and running for 10 seconds,
without any issues, before it’s allowed to start replacing the next one.

```bash
$ kubectl apply -f deploy.yml
```

#### Inspecting Deployments

```bash
$ kubectl get deployments.apps

$ kubectl get deploy hello-deploy
$ kubectl describe deploy hello-deploy

$ kubectl get rs
$ kubectl describe rs hello-deploy-58d896b8d6
```

#### Perform a rolling update

```bash
$ kubectl apply -f deploy.yml

# You can monitor the progress with kubectl rollout status.
$ kubectl rollout status deployment hello-deploy
```

#### Pausing and resuming rollouts

```bash
$ kubectl rollout pause deploy hello-deploy

$ kubectl rollout resume deploy hello-deploy
```

#### Perform a rollback

```bash
$ kubectl rollout history deployment hello-deploy

$ kubectl rollout undo deployment hello-deploy --to-revision=1
```

#### Clean-up

```bash
$ kubectl delete -f deploy.yml
$ kubectl delete -f svc.yml
```
