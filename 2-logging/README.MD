# Logging

Source [https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes)

## Creating a Namespace

A Namespace helps to categorize apps and services inside kubernetes

* get namespaces: `kubectl get namespaces`

* run `kubectl apply -f scripts/1-kube-logging.yaml`

## Creating the Elasticsearch StatefulSet

### Creating the Headless Service

A headless service does not perform load balancing or have a static IP; to learn more about headless services

* run `kubectl apply -f scripts/2-elasticsearch_svc.yaml`

* check if it is created: `kubectl get services --namespace=logging`

### Creating the StatefulSet

A Kubernetes StatefulSet allows you to assign a stable identity to Pods and grant them stable, persistent storage. Elasticsearch requires stable storage to persist data across Pod rescheduling and restarts

* run to check the storage classes available: `kubectl get storageclass`

* run `kubectl apply -f scripts/3-elasticsearch_statefulset.yaml`

* monitor the StatefulSet rollout: `kubectl rollout status sts/es-cluster --namespace=logging`

* `kubectl port-forward service/elasticsearch 9200:9200 --namespace=logging`

* `curl http://localhost:9200/_cluster/state?pretty`

## Creating the Kibana Deployment and Service

To launch Kibana on Kubernetes, we’ll create a Service called kibana, and a Deployment consisting of one Pod replica. You can scale the number of replicas depending on your production needs, and optionally specify a LoadBalancer type for the Service to load balance requests across the Deployment pods.

The ELASTICSEARCH_URL environment variable to set the endpoint and port for the Elasticsearch cluster. Using Kubernetes DNS, this endpoint corresponds to its Service name elasticsearch

* run `kubectl apply -f scripts/4-kibana.yaml`

* `kubectl rollout status deployment/kibana --namespace=logging`

* `kubectl get pods --namespace=logging`

* `kubectl port-forward service/kibana 5601:5601 --namespace=logging`

* [http://localhost:5601](http://localhost:5601)

### Kibana Load Balancer

* run `kubectl apply -f scripts/5-kibana-loadbalancer.yaml`

* `kubectl get svc --namespace=logging`

## Logstash

* run `kubectl apply -f scripts/6-logstash-deployment.yaml`

### Optional

* run `kubectl apply -f scripts/6-logstash-loadbalancer.yaml`

## Creating the Fluentd DaemonSet (extra step log all info from all pods)

* run `kubectl apply -f scripts/7-fluentd.yaml`

* `kubectl get ds --namespace=logging`

* create a kibana Index patterns like `logstash-*`
