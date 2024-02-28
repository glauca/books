### 9: Service discovery deep dive

#### Service registration

The kube-proxy agent on every node is also watching the API server for new EndpointSlice objects. When it sees one, it creates local networking rules on all worker nodes to redirect ClusterIP traffic to Pod IPs. In modern Linux-based Kubernetes clusters, ***the technology used to create these rules is the Linux IP Virtual Server (IPVS). Older versions used iptables.***

#### Service discovery and Namespaces

Cluster address spaces are based on a DNS domain that we call the cluster domain. The domain name is usually cluster.local and objects have unique names within it. For example, a Service in the default Namespace called ent will have a fully qualified domain name (FQDN) of ent.default.svc.cluster.local

```
The format is <object-name>.<namespace>.svc.cluster.local
```

Namespaces let you partition the address space below the cluster domain. For example, creating Namespaces called dev and prod will partition the cluster address space into the following two address spaces:

```
dev: <service-name>.dev.svc.cluster.local

prod: <service-name>.prod.svc.cluster.local”
```

#### Troubleshooting service discovery

+ Pods: Managed by the coredns Deployment
+ Service: A ClusterIP Service called kube-dns listening on port 53 TCP/UDP
+ EndpointSlice objects: Names pre-fixed with kube-dns

#### Command

```
$ kubectl get pods -n kube-system -l k8s-app=kube-dns

$ kubectl get deploy -n kube-system -l k8s-app=kube-dns

$ kubectl get svc -n kube-system -l k8s-app=kube-dns

$ kubectl get endpointslice -n kube-system -l k8s-app=kube-dns

$ cat /etc/resolv.conf

$ kubectl get all --namespace dev

$ kubectl delete pod -n kube-system -l k8s-app=kube-dns
```


```bash
apiVersion: v1
kind: Namespace
metadata:
  name: dev
---
apiVersion: v1
kind: Namespace
metadata:
  name: prod
---
kind: Deployment
apiVersion: apps/v1
metadata:
  name: enterprise
  namespace: dev
spec:
  selector:
    matchLabels:
      app: enterprise
  replicas: 2
  template:
    metadata:
      labels:
        app: enterprise
    spec:
      terminationGracePeriodSeconds: 1
      containers:
      - image: nigelpoulton/k8sbook:text-dev
        name: enterprise-ctr
        ports:
        - containerPort: 8080
---
kind: Deployment
apiVersion: apps/v1
metadata:
  name: enterprise
  namespace: prod
spec:
  selector:
    matchLabels:
      app: enterprise
  replicas: 2
  template:
    metadata:
      labels:
        app: enterprise
    spec:
      terminationGracePeriodSeconds: 1
      containers:
      - image: nigelpoulton/k8sbook:text-prod
        name: enterprise-ctr
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: ent
  namespace: dev
spec:
  ports:
  - port: 8080
  selector:
    app: enterprise
---
apiVersion: v1
kind: Service
metadata:
  name: ent
  namespace: prod
spec:
  ports:
  - port: 8080
  selector:
    app: enterprise
---
apiVersion: v1
kind: Pod
metadata:
  name: jump
  namespace: dev
spec:
  terminationGracePeriodSeconds: 5
  containers:
  - image: ubuntu
    name: jump
    tty: true
    stdin: true

```
