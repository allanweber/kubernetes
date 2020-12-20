# Kubernetes

[https://github.com/kubernetes/examples](https://github.com/kubernetes/examples)

## Running Locally

Use Docker Kubernetes

## Kubectl Common Commands

`kubectl version`

`kubectl cluster-info`

`kubectl get all`

`kubectl get nodes`

`kubectl get pods`

`kubectl get services`

`kubectl run [container-name] --image=[image-name]`

`kubectl port-forward [pod] [ports]` - Forward a port to allow external access

`kubectl expose ...` - Expose a port for a Deployment/Pod

`kubectl create [resource]`

`kubectl apply [resource]` - Create or modify a resource

## Dashboard

Access [https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)

Get the command on **Deploying the Dashboard UI**

`kubectl apply [dashboard-yml-url]`

`kubectl describe secret -n kube-system` - add a token

Copy the very top account token describe as **Type:  kubernetes.io/service-account-token**

`kubectl proxy`

Open the url [http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/) or check in the documentation in the section **Command line proxy**

Apply the token

## Running Pods

`kubectl run [podname] --image=nginx:alpine`

`kubectl port-forward [podname] <external>:<internal>` - Ex.: `kubectl port-forward [podname] 8080:80`

`kubectl delete pod [podname]` - Will recreate the pod

`kubectl delete deployment [podname]` - Delete the deployment that manages the pod (and the pod)

`kubectl apply -f pod.yml` - apply a file

`kubectl get pods --watch` - watch the pods state

`kubectl get pods -o wide` - more info, cluster where the pods are running

`kubectl describe pods hello-pod` = full info

`kubectl get pods --show-labels` - Show labels

`kubectl get pod [POD-NAME] -o yaml` - Get pod info in YAML format

### Testing Connection between PODs

`kubectl exec [POD-NAME] -it sh`

`apk add curl` and `curl -s http://10.2.4.13:8080/health`

`kubectl exec [POD-NAME] -- curl -s http://10.2.4.13:8080/health`

`kubectl exec [POD-NAME] -- curl -s http://[SERVICE-NAME]:[SERVICE-PORT]/health`

## Services

`kubectl get svc` - Get services

`kubectl delete svc [name]` - Delete service

`kubectl expose pod hello-pod --name=hello-svc --target-port=8080 --type=NodePort` - Imperative declaration

`kubectl apply -f file.yml` - Declarative way

### Load Balancer Service

`kubectl apply -f file.yml` - Declarative way

`kubectl get svc` - will show EXTERNAL-IP address to the public internet ip

`kubectl describe ep [service-name]` - Get endpoint of the service, will return in Addresses property the list of health pods IPS that matches the label selector

## Deployment

`kubectl apply -f deploy.yml` - Apply a deployment

`kubectl get deploy` - Get deploys

`kubectl get rs` - Get replica sets

`kubectl get pods` - see deployed running

`kubectl rollout status deploy [deployment-name]` - watch a deployment

`kubectl rollout history deploy web-deploy` - history of rollout

`kubectl rollout undo deploy web-deploy --to-revision=1` - Rollback to some revision

## Getting Started with Kubernetes - Pluralsight (Nigel Poulton)

### Source

[https://github.com/nigelpoulton/getting-started-k8s](https://github.com/nigelpoulton/getting-started-k8s)

[Kubernetes Icons](https://github.com/kubernetes/community/tree/master/icons)

## ConfigMaps

`kubectl get cm [cm-name] -o yaml`

## Secrets

`kubectl get secrets`

`kubectl create secret generic [secret-name] --from-literal=[name]=[value]`

`kubectl get secret [secret-name]`

`kubectl get secret [secret-name] -o yaml`

## Rollback

`kubectl rollout history deployment [name] -n [namespace]`

`kubectl rollout history deployment [name] -n [namespace] --revision=3` - more details

`kubectl rollout undo deployment [name] -n [namespace] --to-revision=3` - undo to revision

## Test Loading

### Use ApacheBench

`ab -n 1000 -c 100 "http://localhost:8080/registration"`

### Curl

`curl http://localhost:8080/registration/?[1-20]`
