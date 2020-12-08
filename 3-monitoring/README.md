# Monitoring

## Create a new namespace

`kubectl apply -f 1-namespace.yml`

## Add Prometheus to Kubernetes with Helm

### Download Prometheus Helm repository

`helm repo add prometheus-community https://prometheus-community.github.io/helm-charts`

### Kubernetes Prometheus deploy

`helm install prometheus stable/prometheus --namespace monitoring`

`helm install prometheus prometheus-community/prometheus --namespace monitoring`

`kubectl get pods --namespace monitoring -l "app=prometheus,component=server" -o jsonpath="{.items[0].metadata.name}"`

`kubectl port-forward prometheus-server-9fc9d497-jd5bl 9090:9090 --namespace=monitoring`

## Modify Deployments to send data to Prometheus

Add the annotations to the deployment

```yaml
..  template:
        metadata:
        labels:
            app: ......
        annotations:
            prometheus.io/scrape: "true"
            prometheus.io/port: "8080"
            prometheus.io/path: "/prometheus"
```

## Add Grafana to Kubernetes with Helm

### Download Grafana Helm repository

`helm repo add grafana https://grafana.github.io/helm-charts`

### Kubernetes Grafana deploy

`helm install grafana-stack grafana/grafana --namespace monitoring`

`kubectl get pods --namespace monitoring -l "app.kubernetes.io/name=grafana-stack,app.kubernetes.io/instance=grafana-stack" -o jsonpath="{.items[0].metadata.name}"`

`kubectl port-forward grafana-stack-665df879b9-zlh8s 3000:3000 --namespace=monitoring`

#### Get your 'admin' user password by running

`kubectl get secret --namespace default my-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo`

### Expose Grafana via LoadBalancer

`kubectl apply -f 2-grafana-loadbalancer.yaml`
