# Kafka Console Consumer CLI

## Table of Contents

- [Basics](#basics)
- [Consumer Group](#consumer-group)

## Basics

```
$ kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic first_topic

```

> `WARNING:` After executing the command, the console consumer only reads from that point when you launch it. It will print a message when a message is produced to the topic.

```
$ kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic first_topic --from-beginning

```

> `WARNING:` After executing the command, the console consumer will read messages from the beginning. Also, it will read messages after they are produced to the topic.

## Consumer Group

```
kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic first_topic --group my-first-application
```

If you remember [this](../readme.md/#consumers), multiple consumers can subscribe to a topic. So you can execute above command concurrently to subscribe multiple consumers to a topic. Each consumer in the group will read a certain partition of the topic.

If you execute more than the partitions of a topic, some of them will be stay inactive until one of them stops working.

```
kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic first_topic --group my-first-application --from-beginning
```

If you execute the command more than one sequentially, the second execution of the console consumer will not print any message because the console consumer committed the offsets and until new messages are produced on the `first_topic` the console consumer will not print any message. This feature of kafka helps recovering from failures and prevents `message duplication`.

# [Next](kafka-consumer-groups)