# Kafka Console Producer CLI

## Table of Contents

- [Producing Messages](#producing-messages)
- [Setting Acknowledgment Mode when Producing Messages](#setting-acknowledgment-mode-when-producing-messages)
- [Producing Messages on a Non-existent Topic](#producing-messages-on-a-non-existent-topic)

## Producing Messages

```
$ kafka-console-producer --broker-list 127.0.0.1:9092 --topic first_topic

> hello world
> awesome
> learning Kafka
> just another message :)
> ^C 		// here you are pressing CTRL+C
```

There are other ways of producing messages.

## Setting Acknowledgment Mode when Producing Messages

```
$ kafka-console-producer --broker-list 127.0.0.1:9092 --topic first_topic --producer-property acks=all

> hello world
> awesome
> learning Kafka
> just another message :)
> ^C 		// here you are pressing CTRL+C
```

## Producing Messages on a Non-existent Topic

```
$ kafka-console-producer --broker-list 127.0.0.1:9092 --topic non_existent_topic_name

> hello world
[TIMESTAMP] WARN [Producer clientId=console-producer] Error while fetching metadata with correlation id 1 : {new_topic=LEADER_NOT_AVAILABLE} (org.apache.kafka.clients.NetworkClient)
> awesome
> ^C 		// here you are pressing CTRL+C
```

> `WARNING:` If you run above command, the topic will still be created and messages will be produced on kafka. However, this could lead to an unwanted situation. The topic will be created with default `partition count` and `replication-factor` which is `1`. Before producing messages check if the topic exists with [list topics command](kafka-topics.md/#list-topics) and if does not exist create the topic with proper configuration.

> `NOTE`: You can also change the default `partition(num.partitions)` and `replication-factor` configuration in `server.properties` file. You have to restart kafka in order to apply the changes.

# [Next](kafka-console-consumer)