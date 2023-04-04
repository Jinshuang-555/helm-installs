# helm-installs
This is the repository to install Kafka/Zookeeper, EFK logging, Prometheus, Grafana and ElasticSearch on Kubernetes Cluster


// ------------------ to install kafka service in the cluster -------------------

$ helm repo add my-repo https://charts.bitnami.com/bitnami
$ helm install my-release my-repo/kafka


// ------------------ to install elastic service in the cluster ----------------

https://phoenixnap.com/kb/elasticsearch-kubernetes

$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm install elasticsearch --set master.replicas=3,coordinating.service.type=LoadBalancer bitnami/elasticsearch

kubectl port-forward --namespace default svc/elasticsearch 9200:9200

curl http://127.0.0.1:9200/

// ---------------- to set up EFK logging stack for the cluster -------------------

$ kubectl apply -f fluentd-rbac.yaml // setup permisions for fluentd daemonset 

$ kubectl apply -f fluentd-daemonset-elasticsearch.yaml

$ helm repo add elastic https://helm.elastic.co

$ helm install kibana elastic/kibana --set elasticsearchHosts=http://localhost:9200

$ kubectl apply -f counter.yaml

Open Kibana dashboard.



