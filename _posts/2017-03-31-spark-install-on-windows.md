---
layout: post
title: windows平台安装spark与hdfs
excerpt: ""
categories: [big-data]
comments: true

---

## 1. 安装jdk

访问http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html，下载最新jdk

当前windows X64最新版本为jdk-8u121-windows-x64.exe

下载完成后，jdk安装到目录 ***C:\Java\jdk1.8.0_121***，jre安装到目录 ***C:\Java\jre1.8.0_121***

> 建议不要安装到Program Files或者Program Files (x86)目录下，目录带有空格或者括号可能影响后面功能使用

配置环境变量**JAVA_HOME**为***C:\Java\jdk1.8.0_121***

配置环境变量**PATH**，添加***%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;***

配置环境变量**CLASSPATH**，添加***.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar***

命令行下执行java -version确认安装成功

    D:>java -version
    java version "1.8.0_121"
    Java(TM) SE Runtime Environment (build 1.8.0_121-b13)
    Java HotSpot(TM) 64-Bit Server VM (build 25.121-b13, mixed mode)

## 2. 安装scala

访问http://www.scala-lang.org/download/all.html，下载所用spark对应的scala版本，比如我这里使用的Spark 1.6.2就只能使用Scala 2.10的各个版本，我们下载2.10.6版本http://downloads.lightbend.com/scala/2.10.6/scala-2.10.6.tgz

解压缩并且文件夹重命名为D:\scala\

> scala安装路径也不能带有空格等非法字符

配置环境变量**SCALA_HOME**为***D:\scala***

配置环境变量**PATH**，添加***%SCALA_HOME%\bin;%SCALA_HOME%\jre\bin;***

配置环境变量**CLASSPATH**，添加***.;%SCALA_HOME%\bin;%SCALA_HOME%\lib\dt.jar;%SCALA_HOME%\lib\tools.jar.;***

命令行下执行scala -version确认安装成功

    D:\>scala -version
    Scala code runner version 2.10.6 -- Copyright 2002-2013, LAMP/EPFL

## 3. 安装hadoop及配置hdfs

访问https://archive.apache.org/dist/hadoop/common/，我们选择hadoop-2.6.4，下载https://archive.apache.org/dist/hadoop/common/hadoop-2.6.4/hadoop-2.6.4.tar.gz

解压缩并重命名为目录D:\hadoop

配置环境变量**HADOOP_HOME**为***D:\scala***

配置环境变量**PATH**，添加***D:\hadoop\bin;***

修改配置文件D:\hadoop\etc\hadoop\core-site.xml

	<configuration>
		<property>
			<name>fs.default.name</name>
			<value>hdfs://localhost:9000</value>
		</property>
	</configuration>

将D:\hadoop\etc\hadoop\mapred-site.xml.template拷贝重命名为mapred-site.xml，并修改

	<configuration>
		<property>
		   <name>mapreduce.framework.name</name>
		   <value>yarn</value>
		</property>
		<property>
		   <name>mapred.job.tracker</name>
		   <value>hdfs://localhost:9001</value>
		</property>
	</configuration>

修改配置文件D:\hadoop\etc\hadoop\hdfs-site.xml，并创建目录D:\hadoop\workplace\data, D:\hadoop\workplace\name

	<configuration>
		<property>
			<name>dfs.replication</name>
			<value>1</value>
		</property>
		<property>
			<name>dfs.data.dir</name>
			<value>D:\hadoop\workplace\data</value>
		</property>
		<property>
			<name>hadoop.tmp.dir</name>
			<value>D:\hadoop\workplace\tmp</value>
		</property>
		<property>
			<name>dfs.name.dir</name>
			<value>D:\hadoop\workplace\name</value>
		</property>
	</configuration>

修改配置文件D:\hadoop\etc\hadoop\yarn-site.xml

	<configuration>

	<!-- Site specific YARN configuration properties -->
		<property>
		   <name>yarn.nodemanager.aux-services</name>
		   <value>mapreduce_shuffle</value>
		</property>
		<property>
		   <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
		   <value>org.apache.hadoop.mapred.ShuffleHandler</value>
		</property>
	</configuration>

修改配置文件D:\hadoop\etc\hadoop\hadoop-env.cmd文件，将JAVA_HOME用 @rem注释掉，编辑为JAVA_HOME的路径

	@rem set JAVA_HOME=%JAVA_HOME%
	set JAVA_HOME=C:\Java\jdk1.8.0_121

访问https://github.com/amihalik/hadoop-common-2.6.0-bin 下载bin目录下的所有文件，并替换D:\hadoop\bin下文件。这里的版本号需要相互对应，否则执行hadoop fs时会提示错误。

确保D:\hadoop\bin\hadoop.cmd保存为windows格式的文件。

运行cmd窗口，执行***hdfs namenode -format***

运行cmd窗口，进入到目录D:\hadoop\sbin，执行命令***start-all.cmd***，将会启动4个窗口执行。命令***stop-all.cmd***终止hadoop。

测试hdfs上传查看文件

	hadoop fs -mkdir hdfs://localhost:9000/user/
	hadoop fs -put a.txt hdfs://localhost:9000/user/a.txt
	hadoop fs -ls hdfs://localhost:9000/user/

hadoop资源管理GUI：http://localhost:8088/

hadoop结点管理GUI：http://localhost:50070/

## 4. 安装spark

访问http://spark.apache.org/downloads.html，Spark release我们选择1.6.2，package type选择Pre-built for hadoop 2.6

下载spark-1.6.2-bin-hadoop2.6.tgz，解压并重名为到目录D:\spark

配置环境变量**PATH**，添加***D:\spark\bin;***

命令行下执行spark-shell命令确认安装成功

    17/03/31 15:59:20 WARN ObjectStore: Failed to get database default, returning NoSuchObjectException
    SQL context available as sqlContext.
    
    scala>

有可能出现如下错误

    17/03/30 11:00:29 WARN ObjectStore: Failed to get database default, returning NoSuchObjectException
    java.lang.RuntimeException: java.lang.RuntimeException: The root scratch dir: /tmp/hive on HDFS should be writable. Current permissions are: --------
    
        at org.apache.hadoop.hive.ql.session.SessionState.start(SessionState.java:522)
        at org.apache.spark.sql.hive.client.ClientWrapper.<init>(ClientWrapper.scala:204)
        at org.apache.spark.sql.hive.client.IsolatedClientLoader.createClient(IsolatedClientLoader.scala:238)

命令行下执行 ***D:\hadoop\bin\winutils.exe chmod 777 D:\tmp\hive*** 后，重新执行spark-shell即可正常

测试spark运行样例SparkPi

	D:\>run-example SparkPi 10
	Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
	17/03/31 16:21:59 INFO DAGScheduler: Job 0 finished: reduce at SparkPi.scala:36, took 3.404880 s
	Pi is roughly 3.13688


