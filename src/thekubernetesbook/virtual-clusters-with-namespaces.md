### 5: Virtual clusters with Namespaces

#### Inspecting Namespaces

```bash
$ kubectl api-resources

# 位于名字空间中的资源
$ kubectl api-resources --namespaced=true
# 不在名字空间中的资源
$ kubectl api-resources --namespaced=false

$ kubectl get namespaces
$ kubectl describe ns default

# You can also add -n or --namespace to regular kubectl commands
# to filter results based on a specific Namespace.

# List all Service objects in the kube-system Namespace.

$ kubectl get svc --namespace kube-system

# You can also use the --all-namespaces flag to return objects from all Namespaces.
```

#### Creating and managing Namespaces

```yaml
kind: Namespace
apiVersion: v1
metadata:
  name: shield
  labels:
    env: marvel
```

```bash
$ kubectl create ns hydra

$ kubectl apply -f shield-ns.yml

$ kubectl get ns

$ kubectl delete ns hydra
```

#### Configuring kubectl to use a specific Namespace

```bash
$ kubectl config set-context --current --namespace shield

# 验证
$ kubectl config view --minify | grep namespace:
```

#### Deploying to Namespaces

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  namespace: shield
  name: default
---
apiVersion: v1
kind: Service
metadata:
  namespace: shield
  name: the-bus
spec:
  type: NodePort
  ports:
  - nodePort: 31112
    port: 8080
    targetPort: 8080
  selector:
    env: marvel
---
apiVersion: v1
kind: Pod
metadata:
  namespace: shield
  name: triskelion
  labels:
    env: marvel
spec:
  containers:
  - image: nigelpoulton/k8sbook:shield-01
    name: bus-ctr
    ports:
    - containerPort: 8080
    imagePullPolicy: Always

```

```bash
$ kubectl apply -f shield-app.yml

$ kubectl get pods -n shield
$ kubectl get svc -n shield
```

#### Clean-up

```bash
$ kubectl delete -f shield-app.yml

$ kubectl delete ns shield

# Reset your kubeconfig so it uses the default Namespace.
# If you don’t do this, future commands will automatically run
# against the deleted shield Namespace which you just deleted.

$ kubectl config set-context --current --namespace default
```
