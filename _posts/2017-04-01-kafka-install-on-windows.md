---
layout: post
title: windows平台安装kafka
excerpt: ""
categories: [big-data]
comments: true

---

## 1. 安装jdk

参考上一篇文章安装

## 2. 安装scala

参考上一篇文章安装

## 3. 安装kafka

访问[http://kafka.apache.org/downloads.html](http://kafka.apache.org/downloads.html)，我们选择2.10-0.10.1.0，下载[https://www.apache.org/dyn/closer.cgi?path=/kafka/0.10.1.0/kafka_2.10-0.10.1.0.tgz](https://www.apache.org/dyn/closer.cgi?path=/kafka/0.10.1.0/kafka_2.10-0.10.1.0.tgz)

解压缩并重命名为目录D:\kafka

修改文件 D:\kafka\config\zookeeper.properties

	dataDir=D:\\kafka\\zookeeper\\db
	clientPort=2181
	maxClientCnxns=0
	dataLogDir=D:\\kafka\\zookeeper\\log

修改文件 D:\kafka\config\server.properties

	advertised.host.name=127.0.0.1
	host.name=127.0.0.1
	auto.create.topics.enable=false
	delete.topic.enable=true
	log.dirs=D:\\kafka\\logs
	listeners=PLAINTEXT://127.0.0.1:9092
	advertised.listeners=PLAINTEXT://127.0.0.1:9092
	zookeeper.connect=localhost:2181

在D:\kafka目录下运行 **bin\windows\zookeeper-server-start.bat config\zookeeper.properties** 启动zookeeper

在D:\kafka目录下运行 **bin\windows\kafka-server-start.bat config\server.properties** 启动broker

在D:\kafka\bin\windows目录下运行**kafka-topics.bat**创建topic

	D:\kafka\bin\windows>kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic hello
	Created topic "hello".
	D:\kafka\bin\windows>kafka-topics.bat --list --zookeeper localhost:2181
	hello

在D:\kafka\bin\windows目录下运行**kafka-console-producer.bat**往topic发消息

	D:\kafka\bin\windows> kafka-console-producer.bat --broker-list localhost:9092 --topic hello
	hello
	world
在D:\kafka\bin\windows目录下运行**kafka-console-consumer.bat**从topic收消息

	D:\kafka\bin\windows>kafka-console-consumer.bat --zookeeper localhost:2181 --topic  hello --from-beginning
	Using the ConsoleConsumer with old consumer is deprecated and will be removed in a future major release. Consider using the new consumer by passing [bootstrap-server] instead of [zookeeper].
	hello
	world