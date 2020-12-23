# Ingress

## Useful commands

`k describe ingress [ingress-name]`

## Ingress install

[https://www.linode.com/docs/guides/how-to-configure-load-balancing-with-tls-encryption-on-a-kubernetes-cluster](https://www.linode.com/docs/guides/how-to-configure-load-balancing-with-tls-encryption-on-a-kubernetes-cluster)

### Install Helm

```bash
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh
```

### (Optional) Add sample applications

`k apply -f sample/hello-one.yaml`

`k apply -f sample/hello-two.yaml`

### Install the NGINX Ingress Controller

`helm repo add stable https://charts.helm.sh/stable`

`helm repo update`

`helm install nginx-ingress stable/nginx-ingress`

`kubectl --namespace default get services -o wide -w nginx-ingress-controller`

#### DNS Config

**Add A record to DNS confing on the host provider and than check the propagation with some of the commands below**

`nslookup what.domainexample.com 8.8.8.8`

`nslookup what.domainexample.com`

`dig +short what.domainexample.com`

### Create a TLS Certificate Using cert-manager

1. Install cert-manager’s CRDs.

 `kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v0.15.1/cert-manager.crds.yaml`

2. Create a cert-manager namespace.

`kubectl create namespace cert-manager`

3. Add the Helm repository which contains the cert-manager Helm chart.

`helm repo add jetstack https://charts.jetstack.io`

4. Update your Helm repositories.

`helm repo update`

5. Install the cert-manager Helm chart

`helm install cert-manager jetstack/cert-manager --namespace cert-manager --version v0.15.1`

5. Verify that the corresponding cert-manager pods are now running.

`kubectl get pods --namespace cert-manager`

**You should wait until all cert-manager pods are ready and running prior to proceeding to the next section.**

### Create a ClusterIssuer Resource

`kubectl apply -f acme-issuer-prod.yaml`

`kubectl get clusterissuer -o wide`

### Enable HTTPS for your Application

`kubectl apply -f sample/hello-app-ingress.yaml`

`kubectl get ingress`

`kubectl get ingress --all-namespaces -o wide`

## Single Service V1.18

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: ingress-single
  annotations:
    # kubernetes.io/ingress.class: nginx ????
spec:
  backend:
    service:
        serviceName: hello-world-service-single
        servicePort: 8080
```

## Single Service V1.19

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-single
spec:
  defaultBackend:
    service:
        name: hello-world-service-single
        port: 
            number: 8080
```

## Path Services V1.18

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: ingress-path
  # kubernetes.io/ingress.class: nginx ????
spec:
  rules:
    - host: path.example.com
      http:
        paths:
        - path: /red
          backend:
            serviceName: hello-world-service-red
            servicePort: 4242
        - path: /blue
          backend:
            serviceName: hello-world-service-blue
            servicePort: 4243
#   default route
  backend: 
    serviceName: hello-world-service-single
    servicePort: 8080
```

## Path Services V1.19

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-path
spec:
  rules:
    - host: path.example.com
      http:
        paths:
        - path: /red
          pathType: Prefix
          backend:
            service:
              name: hello-world-service-red
              port: 
                number: 4242
        - path: /blue
          pathType: Prefix
          backend:
            service:
              name: hello-world-service-blue
              port: 
                number: 4343
  defaultBackend:
    service:
        name: hello-world-service-single
        port: 
            number: 80
```

## Named Based Virtual Hosts v1.18

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: ingress-namebased
spec:
  rules:
  - host: red.example.com
    http:
      paths:
      - backend:
        serviceName: hello-world-service-red
        servicePort: 4242
  - host: blue.example.com
    http:
      paths:
      - backend:
        serviceName: hello-world-service-blue
        servicePort: 4243
```

## Named Based Virtual Hosts v1.19

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-namebased
spec:
  rules:
  - host: red.example.com
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: hello-world-service-red
            port:
              number: 4242
  - host: blue.example.com
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: hello-world-service-blue
            port:
              number: 4343
```







## Using TLS

```yaml
...
spec:
  tls:
  - hosts:
      - tls.example.com
    secretName: tls-secret   
```