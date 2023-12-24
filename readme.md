clone https://github.com/confluentinc/cp-helm-charts in helm folder

Install helm. Then install kafka

> helm install local-confluent-kafka /helm/cp-helm-charts/ --version 0.6.0

(see https://github.com/confluentinc/cp-helm-charts)

See that kafka run:

> kubectl get pods

Update helms/cp-helm-charts/charts/cp-zookeeper/templates/poddisruptionbudget.yaml
`apiVersion: policy/v1beta` to `apiVersion: policy/v1`

Run kafka client
> kubectl apply -f kafka-client.yml


## Enter kafka pod cli

kubectl exec -it kafka-client -- /bin/bash

## Create topics in kafka pod cli (see also create-topics.sh)
kafka-topics --zookeeper local-confluent-kafka-cp-zookeeper-headless:2181 --topic customer --create --partitions 3 -replication-factor 3 --if-not-exists

kafka-topics --zookeeper local-confluent-kafka-cp-zookeeper-headless:2181 --topic payment-request --create --partitions 3 -replication-factor 3 --if-not-exists

Required topics list:
```
customer
payment-request
payment-response
restaurant-approval-request
restaurant-approval-response
```


### See topics
kafka-topics --zookeeper local-confluent-kafka-cp-zookeeper-headless:2181 --list

helm uninstall local-confluent-kafka

kubectl delete -f kafka-client.yml

# run kafka pods in /helm
helm install local-confluent-kafka cp-helm-charts --version 0.6.0

# run postgres
kubectl apply -f postgres-deployment.yml

# run services
kubectl apply -f application-deployment-local.yml

# stop services
kubectl delete -f application-deployment-local.yml
kubectl get deployments
kubectl delete deployment <deployment-name>

