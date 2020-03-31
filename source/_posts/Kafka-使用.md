---
title: Kafka 使用
date: 2020-08-06 08:42:57
tags:
---

# Kafka



<!--more-->

```
ZK=10.1.1.60:2181
KA=10.1.1.60:9092
```



创建 topic

```
./kafka-topics.sh --create --topic test1 --partitions 4 --zookeeper $ZK --replication-factor 1
```



查看 topic

```
./kafka-topics.sh --list --zookeeper $ZK
```



生成消息

```
./kafka-console-producer.sh --topic=test --broker-list=$KA
```



消费消息

```
./kafka-console-consumer.sh --topic=test --bootstrap-server $KA
```