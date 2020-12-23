# Kubernetes with 1clickinfra

[https://1clickinfra.com/](https://1clickinfra.com/)

## Apply the new kubectl config file

`export KUBECONFIG=/home/<user>/.kube/kubeconfig-file.yaml`

## Grafana Port Forward

`kubectl port-forward svc/grafana --namespace infrastructure 8888:80`

### Get Grafana Password

`kubectl get secret grafana --namespace infrastructure --template='{{ index .data "admin-password"}}' | base64 -d`
