# Problems and question for kafka docker usage

### Failed to produce event to kafka
The problem is caused by using environment variable KAFKA_CREATE_TOPICS in the docker-compose.yml.  
Don't know why, but after configure KAFKA_CREATE_TOPICS, seems the topic is created.  
But when try to produce some message, error will be reported. Try to create the topic manually for now.  
We can create topic manually by the following command:
```bash
# This command is running in the host
docker-compose exec kafka bash

# after previous command, we are in kafka
$KAFKA_HOME/bin/kafka-topics.sh --create --zookeeper zookeeper:2181 --replication-factor 1 --partitions 1 --topic mytopic  

```

### Produce events to kafka by command

```bash
# This command is running in the host
docker-compose exec kafka bash

# after previous command, we are in kafka
$KAFKA_HOME/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic mytopic  
```

### Delete a topic in kafka by command
```bash
# This command is running in the host
docker-compose exec kafka bash

# after previous command, we are in kafka
$KAFKA_HOME/bin/kafka-topics.sh --delete --zookeeper zookeeper:2181 --topic mytopic

# after previous command, we should have deleted the topic if there is no data transfered
# In order to delete the topic in that case, we need to delete the kafka data and zookeeper data
$KAFKA_HOME/bin/zookeeper-shell.sh

# after previous command, we are in zookeeper shell, run below command to delete the topic from zookeeper
rmr  /brokers/topics/mytopic

# find where the kafka data is stored, example log.dirs=/kafka/kafka-logs-00c386f8d753
cat config/server.properties  | grep log.dirs

# remove kafka data
rm -rf /kafka/kafka-logs-00c386f8d753/mytopic*
```

### List all the topic in kafka by command
```bash
# This command is running in the host
docker-compose exec kafka bash

# after previous command, we are in kafka
$KAFKA_HOME/bin/kafka-topics.sh --list --zookeeper zookeeper:2181
```

### Consume a topic in kafka by command
```bash
# This command is running in the host
docker-compose exec kafka bash

# after previous command, we are in kafka
$KAFKA_HOME/bin/kafka-console-consumer.sh --zookeeper zookeeper:2181 --topic mytopic --from-beginning
```
