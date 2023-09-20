ingress

```bash
minikube start -n 2
minikube addons enable metallb
minikube addons enable ingress

kubectl apply -f metallb-config.yaml

kubectl edit configmap -n kube-system kube-proxy
kubectl --namespace kube-system delete pod --selector='k8s-app=kube-proxy'

kubectl apply -f web-deploy.yaml
kubectl apply -f web-svc-headless.yaml
kubectl apply -f nginx-lb.yaml
kubectl apply -f web-ingress.yaml
```

```bash
kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   16m
web-svc      ClusterIP   None         <none>        80/TCP    5m16s
```

```bash
kubectl get ingresses.networking.k8s.io
NAME          CLASS   HOSTS   ADDRESS        PORTS   AGE
web-ingress   nginx   *       192.168.49.2   80      10m
```

```bash
kubectl get endpoints
NAME         ENDPOINTS                                                    AGE
kubernetes   192.168.49.2:8443                                            20m
web-svc      10.244.120.71:8000,10.244.205.197:8000,10.244.205.198:8000   9m7s
```

```bash
kubectl describe ingress/web-ingress
Name:             web-ingress
Labels:           <none>
Namespace:        default
Address:          192.168.49.2
Ingress Class:    nginx
Default backend:  <default>
Rules:
  Host        Path  Backends
  ----        ----  --------
  *
              /   web-svc:8000 (10.244.120.71:8000,10.244.205.197:8000,10.244.205.198:8000)
Annotations:  nginx.ingress.kubernetes.io/rewrite-target: /
Events:
  Type    Reason  Age                    From                      Message
  ----    ------  ----                   ----                      -------
  Normal  Sync    7m57s (x2 over 8m43s)  nginx-ingress-controller  Scheduled for sync
```

