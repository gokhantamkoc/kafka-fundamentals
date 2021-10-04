# Kafka Consumer Group CLI

## Table of Contents

- [Basics](#basics)
- [Resetting Offsets](#resetting-offsets)

## Basics

```
$ kafka-consumer-groups --bootstrap-server localhost:9092 --list
my-first-application
my-second-application
console-consumer-10922
```

```
$ kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group my-second-application
Consumer group 'my-second-application' has no active members

TOPIC		PARTITION		CURRENT-OFFSET		LOG-END-OFFSET		LAG		CONSUMER-ID	HOST		CLIENT-ID
first_topic	1			21				21				0		-			-		-
first_topic	0			22				22				0		-			-		-
first_topic	2			20				20				0		-			-		-
```

- `LAG` zero means all the data is read on the partition by consumer group `my-second-application`.
- If you run consumer group `my-second-application`, the `CONSUME-ID`, `HOST`, `CLIENT-ID` will be filled with necessary info and `Consumer group 'my-second-application' has no active members` message will no longer appear.

## Resetting Offsets

```
$ kafka-consumer-groups --bootstrap-server localhost:9092 --group my-first-application --reset-offsets --to-earliest --topic first_topic

TOPIC		PARTITION		NEW-OFFSET
first_topic	1			0
first_topic	0			0
first_topic	2			0
```

`--reset-offsets` resets the offsets to the beginning

```
$ kafka-consumer-groups --bootstrap-server localhost:9092 --group my-first-application --reset-offsets --shift-by -2 --topic first_topic

TOPIC		PARTITION		NEW-OFFSET
first_topic	1			19
first_topic	0			22
first_topic	2			18
```

`--shift-by -2` resets back the offsets of all partitions by 2.