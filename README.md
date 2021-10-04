# Kafka Fundamentals

## **Table of Contents**

- [Topics and Partitions](#topics-and-partitions)
- [Brokers](#brokers)
- [Producers](#producers)
- [Message Keys](#message-keys)
- [Consumers](#consumers)
	- [Consumer Offsets](#consumer-offsets)
	- [Delivery Semantics](#delivery-semantics)
- [Broker Discovery](#broker-discovery)
- [Zookeeper](#zookeeper)
- [Kafka Guarantees](#kafka-guarantees)


## **Theory of Kafka**

### **Topics and Partitions**

- A topic is a particular Stream of data. It is similar to table in a DB. It is defined by name.

- Topics are split into partitions

```
			 ---- Partition 0
			/
Kafka Topic ------ Partition 1
			\
			 ---- Partition 2
```

- Each partition is ordered.
- Each message within a partition gets an `incremental id`. This `incremental id` is called `offset`.

```
Message
 _/\_
/    \
|  0  |  1  |  2  |  ...  |  N  |
\_____________  ________________/
		      \/
			Partition
```

- Offset only have a meaning for a specific partition.
- Order is only guaranteed within a partition.
- Data is kept only for a limited time. (Default is a week)
- Once the data is written to a partition, it `CANNOT` be changed.
- Data assigned randomly to a partition unless a key is provided.

### **Brokers**

- A kafka cluster is composed of multiple brokers (servers/containers/pods)
- Brokers holds the topic and its data.
- Each is identified by its integer ID.
- After connecting to a broker (called a bootstrap broker), you will be connected to the cluster.
- 3 brokers is good to start a kafka cluster, because of the `quorum attribute`.

> In-Sync Replica(ISR): Helps maintaining high availability and consistency. Three brokers in a kafka cluster is the best for using ISR in production.

### **Producers**

How do we write data to Kafka?
- Producers write data to topics.
- Producers automatically know which broker and partition to write.
- In case of broker failure, Producers will automatically recover.

How producers write data?
- Acknowledgements of data writes: There are 3 modes for it:
	1. *acks=0*: Do `NOT` wait for acknowledgement. (Possible data loss)
	2. *acks=1*: Wait for Leader acknowledgement. (Limited data loss)
	3. *acks=all*: Wait for both Leader and ISR acknowledgment. (No data loss)

### Message Keys

Producers can send a key but it is **optional** with the message. It can be string, integer etc...
- if key = null, data is sent by round robin.
- if key is `NOT` null, then all messages for that will always go to the same partition.

### **Consumers**

- Consumers read data from a topic.
- Consumers know which broker to read from.
- In case of broker failure, Consumers know how to recover.
- Data is read in order within each partition.
- If you more consumers than partitions, some consumers will be inactive.

> If you have the resources, it is a good practice to have more consumers than partitions. You will ensure high availability for consuming data.

#### **Consumer Offsets**

- Kafka stores the offsets at which a consumer group has been reading.
- The offsets committed live in a kafka topic named: `__consumer_offsets`
- When a consumer in a group has processed data received from kafka, it should be committing the offsets. Why?
	- if a consumer dies, it will be able to read back from where it left off thanks to the **committed consumer offsets**!

#### **Delivery Semantics**

There are 3 delivery semantics for a Consumer:

1. *At most once*: offsets are committed as soon as the message is received. So if the processing goes wrong, the message will be lost. Hence, it is not usually preferred.
2. *At least once*: offsets are committed after the message is processed. So if the processing goes wrong, this message will be read again. (Same as `Dead letter` in RabbitMQ). This is usually preferred. This may result in duplicate processing of messages. Make sure your processing is `idempotent` meaning not affecting your system. 
3. *Exactly once*: can be achieved with **Kafka to Kafka Workflows** using `Kafka Streams API`.

### **Broker Discovery**

- Every broker is also called `bootstrap-server`. That means when you connect to one broker you will be connected to the entire cluster.
- Each broker knows about all brokers, topics and partitions(metadata requests).
- The process between Kafka Client(Producer or consumer) and Kafka Cluster:
	1. Connection.
	2. Metadata Request.
	3. Retrieves all brokers info.
	4. Connect to needed brokers.

### **Zookeeper**

- Manages brokers.
- Helps in performing leader selection for partitions.
- Zookeeper informs kafka in case changes like new topic, broker dies, broker comes up, delete topics... etc. occur.

> `WARNING:` In order to run kafka without Zookeeper, use Kafka Raft metadata mode ( KRaft ). In KRaft the kafka metadata mode, information will be stored as a partition within kafka itself. There will be a KRaft Quorum of controller nodes which will be used to store the metadata. The metadata will be stored in an internal kafka topic @metadata.

- By design, Zookeeper works with an odd number of servers (3,5,7...)
- Zookeeper has leader/follower concept as well. Leader handles the `writes` and followers handle the `reads`.

### **Kafka Guarantees**

- Messages are appended to `topic-partition` in the order they are sent.
- Consumers read messages in the order stored in a `topic-partition`.
- With a `replication factor of N`, producers and consumers can tolerate up to `N-1 brokers down`.

```
Replication Factor - 1 = # of Brokers can be down
```

> This is why a replication factor 3 is a good idea:

- Allows for one broker to be taken for `maintenance`.
- Allows for another broker to be taken down `unexpectedly`.

- As long as, the # of partitions remains constant(no new partitions) for a topic, the same key will always go to the same partition.

- The `replication factor` **CANNOT** be more than # available brokers.

# [Next](CLI/kafka-topics)