# usefulcontainers

Plain Bash

    kubectl run --it --rm  --image bash bash




Amazon Cli

    docker run --it --rm --entrypoint /bin/sh amazon/aws-cli:2.0.55


To connect to your database run the following command:

    kubectl run gait-astro-dev-db-postgresql-client --rm --tty -i --restart='Never' --namespace default --image docker.io/bitnami/postgresql:9.6 --env="PGPASSWORD=$POSTGRES_PASSWORD" --command -- psql --host postgresql -U postgres -p 5432
    
     kubectl run mysql -it --rm --image=mysql:5.6 --restart=Never --command bash
     
     
     
     
Kakfa Send and receive messages


    Once the cluster is running, you can run a simple producer to send messages to a Kafka topic (the topic will be automatically created):

    kubectl -n kafka run kafka-producer -ti --image=quay.io/strimzi/kafka:0.23.0-kafka-2.8.0 --rm=true --restart=Never -- bin/kafka-console-producer.sh --broker-list my-cluster-kafka-bootstrap:9092 --topic my-topic
  
  
    And to receive them in a different terminal you can run:

    kubectl -n kafka run kafka-consumer -ti --image=quay.io/strimzi/kafka:0.23.0-kafka-2.8.0 --rm=true --restart=Never -- bin/kafka-console-consumer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic my-topic --from-beginning
    
    
    
Busybox Curl 

     kubectl run busybox-curl --image=yauritux/busybox-curl --restart=Never --command -- sleep 3600

Custom Script to restart Strimzi containers

```
        
# Get the list of connectors for New relic and loop through each of them to find the failed connectors.
 
for connector in $(kubectl get kafkaconnector| awk '{print $1}')
do
 
# Finding the failed conncetors
state=$(kubectl get kafkaconnector $connector -ojson | jq -r .status.connectorStatus.tasks | jq  '.[0]' | jq -r .state)
echo $state
 
# Lets restart the failed connectors from command line by fetching the pod ip and executing the restart commands
 
if [[ $state == "FAILED" ]]; then
 
echo "I got a failure"
 
ip=$(kubectl get kafkaconnector $connector -ojson | jq -r .status.connectorStatus.tasks | jq  '.[0]'| jq -r .worker_id | awk -F ":" '{print $1}')
pod_name=$(kubectl get pods -owide | grep -i $ip | awk '{print $1}')
kubectl exec -it $pod_name -- curl http://localhost:8083/connectors/$connector/status
kubectl exec -it $pod_name -- curl -X POST http://localhost:8083/connectors/$connector/tasks/0/restart
 
# Lets give it sometime to recover from restart and then check the status.
sleep 3
kubectl exec -it $pod_name -- curl http://localhost:8083/connectors/$connector/status
 
fi
 
 
done

```


    
