
```bash
helm repo add stable https://charts.helm.sh/stable
helm repo list

NAME         	URL
jetstack     	https://charts.jetstack.io
chartmuseum  	https://chartmuseum.github.io/charts
ingress-nginx	https://kubernetes.github.io/ingress-nginx
stable       	https://charts.helm.sh/stable
```
```bash
kubectl create ns nginx-ingress
```
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx

NAME: ingress-nginx
LAST DEPLOYED: Sun Oct  8 20:27:05 2023
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
The ingress-nginx controller has been installed.
It may take a few minutes for the LoadBalancer IP to be available.
You can watch the status by running 'kubectl --namespace default get services -o wide -w ingress-nginx-controller'

An example Ingress that makes use of the controller:
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: example
    namespace: foo
  spec:
    ingressClassName: nginx
    rules:
      - host: www.example.com
        http:
          paths:
            - pathType: Prefix
              backend:
                service:
                  name: exampleService
                  port:
                    number: 80
              path: /
    # This section is only required if TLS is to be enabled for the Ingress
    tls:
      - hosts:
        - www.example.com
        secretName: example-tls

If TLS is enabled for the Ingress, a Secret containing the certificate and key must also be provided:

  apiVersion: v1
  kind: Secret
  metadata:
    name: example-tls
    namespace: foo
  data:
    tls.crt: <base64 encoded cert>
    tls.key: <base64 encoded key>
  type: kubernetes.io/tls
```
```bash
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.9.1/cert-manager.yaml
```
```bash
kubectl get pods -n cert-manager

NAME                                      READY   STATUS    RESTARTS   AGE
cert-manager-6dcb4b7545-k68x2             1/1     Running   0          35s
cert-manager-cainjector-d9f7668c6-kpzn6   1/1     Running   0          35s
cert-manager-webhook-fb98c48b5-4jpg4      1/1     Running   0          35s
```
```bash
kubectl create ns chartmuseum
```
```bash
helm install my-chartmuseum chartmuseum/chartmuseum --namespace=chartmuseum --wait --version 3.1.0 -f kubernetes-templating/chartmuseum/values.yaml

W1008 20:58:13.421226  327431 warnings.go:70] annotation "kubernetes.io/ingress.class" is deprecated, please use 'spec.ingressClassName' instead
NAME: my-chartmuseum
LAST DEPLOYED: Sun Oct  8 20:58:11 2023
NAMESPACE: chartmuseum
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
** Please be patient while the chart is being deployed **

Get the ChartMuseum URL by running:

  export POD_NAME=$(kubectl get pods --namespace chartmuseum -l "app=chartmuseum" -l "release=my-chartmuseum" -o jsonpath="{.items[0].metadata.name}")
  echo http://127.0.0.1:8080/
  kubectl port-forward $POD_NAME 8080:8080 --namespace chartmuseum
```
```bash
helm ls --namespace chartmuseum

NAME          	NAMESPACE  	REVISION	UPDATED                                	STATUS  	CHART            	APP VERSION
my-chartmuseum	chartmuseum	1       	2023-10-08 20:58:11.476624264 +0300 MSK	deployed	chartmuseum-3.1.0	0.13.1
```
```bash
kubectl get secrets --namespace chartmuseum

NAME   TYPE                 DATA   AGE
chartmuseum.51.250.70.204.sslip.io-xkxqf   Opaque               1      72s
my-chartmuseum                             Opaque               0      72s
sh.helm.release.v1.my-chartmuseum.v1       helm.sh/release.v1   1      72s
```