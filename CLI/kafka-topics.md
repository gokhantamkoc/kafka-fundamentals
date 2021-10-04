# Kafka Topics CLI

## Table of Contents

- [Create a Topic](#create-a-topic)
- [List Topics](#list-topics)
- [Details of a Topic](#details-of-a-topic)

## Create a Topic

```
$ kafka-topics --zookeeper 127.0.0.1:2181 --topic first_topic --create --partitions 3 --replication-factor 1
```

> `WARNING:` `--replication-factor` cannot be larger than # available brokers.

## List Topics

```
$ kafka-topics --zookeeper 127.0.0.1:2181 --list
```

## Details of a Topic

```
$ kafka-topics --zookeeper 127.0.0.1:2181 --topic --describe
```

## Delete a Topic

```
$ kafka-topics --zookeeper 127.0.0.1:2181 --topic second_topic --delete
```

> `WARNING:` Kafka does `NOT` immediately deletes the topic. The topic will `marked for deletion`. Until all producers and consumers unsubscribe to the topic, the topic will `NOT` be deleted.

# [Next](kafka-console-producer)