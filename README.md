# helm-installs
This is the repository to install Kafka/Zookeeper, EFK logging, Prometheus, Grafana and ElasticSearch on Kubernetes Cluster


// ------------------ to install kafka service in the cluster -------------------

$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm install my-release bitnami/kafka


// ------------------ to install elastic service in the cluster ----------------

https://phoenixnap.com/kb/elasticsearch-kubernetes

$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm install elasticsearch --set master.replicas=1,coordinating.service.type=LoadBalancer,xpack.security.enrollment.enabled=true,security.tls.restEncryption=true bitnami/elasticsearch

helm install elasticsearch --set security.tls.restEncryption=true,global.kibanaEnabled=true,global.elasticsearch.service.name=elasticsearch,global.elasticsearch.service.ports.restAPI=9200 bitnami/elasticsearch

kubectl port-forward service/elasticsearch 9200:9200

// ---------------- to set up EFK logging stack for the cluster -------------------

helm repo update  

<!-- $ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm install kibana bitnami/kibana \
  --set 'elasticsearch.hosts[0]=elasticsearch-coordinating-hl' \
  --set elasticsearch.port=9200 \
  --set kibana.elasticsearch.security.tls.enabled=true -->

helm install kibana bitnami/kibana \
  --set 'elasticsearch.hosts[0]=elasticsearch' \
  --set elasticsearch.port=9200 \
  --set kibana.elasticsearch.security.tls.enabled=true

Open Kibana dashboard.

kubectl port-forward service/kibana 5601:5601

kubectl port-forward service/elasticsearch-kibana 5601:5601

localhost:5601

$ kubectl apply -f fluentd-rbac.yaml // setup permisions for fluentd daemonset 

kubectl apply -f fluentd-config.yaml

$ kubectl apply -f fluentd-daemonset-elasticsearch.yaml

kubectl apply -f counter.yaml




helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

helm install prometheus prometheus-community/kube-prometheus-stack

kubectl get services

kubectl port-forward service/prometheus-operated 9090:9090

kubectl port-forward service/prometheus-grafana 3000:80

