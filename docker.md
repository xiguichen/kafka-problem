### Failed to produce event to kafka
The problem is caused by using environment variable KAFKA_CREATE_TOPICS in the docker-compose.yml.  
Don't know why, but after configure KAFKA_CREATE_TOPICS, seems the topic is created.  
But when try to produce some message, error will be reported. Try to create the topic manually for now.  
We can create topic manually by the following command:
```bash
# This command is running in the host
docker-compose exec kafka bash

# after previous command, we are in kafka
/opt/kafka/bin/kafka-topics.sh --create --zookeeper zookeeper:2181 --replication-factor 1 --partitions 1 --topic mytopic  

```

### Produce events to kafka by command

```bash
# This command is running in the host
docker-compose exec kafka bash

# after previous command, we are in kafka
/opt/kafka/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic mytopic  
```
