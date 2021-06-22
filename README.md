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



    
