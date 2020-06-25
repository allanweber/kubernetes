# Kubernetes

## Running Locally

Use Docker Kubernetes

## Kubectl Common Commands

`kubectl version`

`kubectl cluster-info`

`kubectl get all`

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

