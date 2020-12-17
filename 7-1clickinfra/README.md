# Kubernetes with 1clickinfra

[https://1clickinfra.com/](https://1clickinfra.com/)

## Apply the new kubectl config file

`export KUBECONFIG=/home/<user>/.kube/kubeconfig-file.yaml`

## Get NGINX Controller load balancer

`k get svc -n infrastructure` - get  EXTERNAL-IP of nginx-nginx-ingress-controller

Add it as A record of the DNS with the **'*'**  wildcard

`nslookup grafana.candidates-career.tech && nslookup grafana.candidates-career.tech 8.8.8.8` - to check if the DNS is already replicated

It can be also checked on some DNS checker [https://dnschecker.org/](https://dnschecker.org/) for the address grafana.candidates-career.tech for example

## Check certificate issuer

It takes a while to be provisioned

`k get clusterissuer -n infrastructure`
