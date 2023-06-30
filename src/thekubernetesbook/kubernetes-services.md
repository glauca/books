### 7: Kubernetes Services

```yaml
apiVersion: v1
kind: Service
metadata:
  name: dual-stack-svc
spec:
  ipFamilyPolicy: PreferDualStack # Assign IPv4 and IPv6 ClusterIPs
  ipFamilies:
  - IPv6
  - IPv4
  type: ClusterIP
  ports:
  - port: 8080
    protocol: TCP
  selector:
    app: hello-world
```

> The field of interest is **.spec.ipFamilyPolicy** which can be set to any of the following
three values:
• SingleStack
• PreferDualStack
• RequireDualStack

> You can control whether the IPv4 or IPv6 address is the primary ClusterIP by specifying
the order in **.spec.ipFamilies**. The first family in the list will be the primary ClusterIP
and therefore the value returned by kubectl get.

```bash
$ kubectl apply -f dual-stack-svc.yml
$ kubectl get svc
$ kubectl describe svc dual-stack-svc
```

#### The imperative way

```bash
$ kubectl expose deployment svc-test --type=NodePort
$ kubectl get svc -o wide
$ kubectl delete svc svc-test
```

> By default, cluster-wide NodePorts are between 30,000 - 32,767.

#### The declarative way

```yaml
apiVersion: v1
kind: Service
metadata:
  name: svc-test
  labels:
    chapter: services
spec:
# ipFamilyPolicy: PreferDualStack
# ipFamilies:
# - IPv4
# - IPv6
  type: NodePort
  ports:
  - port: 8080
    nodePort: 30001
    targetPort: 8080
    protocol: TCP
  selector:
    chapter: services
```

#### EndpointSlice objects

```bash
$ kubectl get endpointslices
$ kubectl describe endpointslice svc-test-n7jg4
```

#### LoadBalancer Services

```yaml
apiVersion: v1
kind: Service
metadata:
  name: cloud-lb
spec:
  type: LoadBalancer
  ports:
  - port: 9000
    targetPort: 8080
  selector:
    chapter: services
```
