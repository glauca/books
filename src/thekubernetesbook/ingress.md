### 8: Ingress

#### Installing the NGINX Ingress controller

latest release (see [https://github.com/kubernetes/ingress-nginx](https://github.com/kubernetes/ingress-nginx))

```
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.1/deploy/static/provider/cloud/deploy.yaml

$ kubectl get pods -n ingress-nginx -w

$ kubectl get ingressclass

$ kubectl describe ingressclass nginx

$ kubectl get ing
```

#####  app.yml

```ymal
apiVersion: v1
kind: Service
metadata:
  name: svc-shield
spec:
  type: ClusterIP
  ports:
  - port: 8080
    targetPort: 8080
  selector:
    env: shield
---
apiVersion: v1
kind: Service
metadata:
  name: svc-hydra
spec:
  type: ClusterIP
  ports:
  - port: 8080
    targetPort: 8080
  selector:
    env: hydra
---
apiVersion: v1
kind: Pod
metadata:
  name: shield
  labels:
    env: shield
spec:
  containers:
  - image: nigelpoulton/k8sbook:shield-ingress
    name: shield-ctr
    ports:
    - containerPort: 8080
    imagePullPolicy: Always
---
apiVersion: v1
kind: Pod
metadata:
  name: hydra
  labels:
    env: hydra
spec:
  containers:
  - image: nigelpoulton/k8sbook:hydra-ingress
    name: hydra-ctr
    ports:
    - containerPort: 8080
    imagePullPolicy: Always
```

##### ig-mcu-host.yml

```ymal
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: mcu-host
spec:
  ingressClassName: nginx
  rules:
  - host: shield.mcu.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: shield
            port:
              number: 8080
  - host: hydra.mcu.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: hydra
            port:
              number: 8080
```

##### ig-mcu-path.yml

```ymal
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: mcu-paths
spec:
  rules:
  - host: mcu.com
    http:
      paths:
      - path: /shield
        pathType: Prefix
        backend:
          service:
            name: shield
            port:
              number: 8080
      - path: /hydra
        pathType: Prefix
        backend:
          service:
            name: hydra
            port:
              number: 8080
```

##### ig-all.yml

```ymal
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: mcu-all
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: shield.mcu.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: svc-shield
            port:
              number: 8080
  - host: hydra.mcu.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: svc-hydra
            port:
              number: 8080
  - host: mcu.com
    http:
      paths:
      - path: /shield
        pathType: Prefix
        backend:
          service:
            name: svc-shield
            port:
              number: 8080
      - path: /hydra
        pathType: Prefix
        backend:
          service:
            name: svc-hydra
            port:
              number: 8080
```

#### Clean-up

```
$ kubectl delete ingress mcu-all

$ kubectl delete namespace ingress-nginx

$ kubectl delete clusterrole ingress-nginx

$ kubectl delete clusterrolebinding ingress-nginx
```
