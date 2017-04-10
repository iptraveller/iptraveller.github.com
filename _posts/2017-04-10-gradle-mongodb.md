---
layout: post
title: IDEA创建gradle工程连接mongodb
excerpt: ""
categories: [big-data]
comments: true

---

## 1. Intellij IDEA创建gradle工程

New -> Project -> Gradle ，勾选Java，选择Next；

GroupId和ArtifactId随便填，选择Next；

勾选“Create directories for empty content roots automatically”，选择“Use local gradle distribution”填入本地gradle路径，选择Next；

点击Finish。

创建完工程后，等工程自动刷新完。

刷新完后就能看到工程目录结构，自动创建了src/main/java

## 2. 添加mongodb库

修改build.gradle

    dependencies {
        compile group: 'org.mongodb', name: 'mongo-java-driver', version: '3.4.1'
    }

刷新gradle，可以在External Libraies中看到mongo-java-driver

## 3.代码连接mongodb 

在src/main/java下创建一个package并添加一个class

    public class MongoExample {
        public static void main(String args[]) {
            System.out.println("helloworld");
            try {
                MongoClient mongoClient = new MongoClient("localhost", 27017);
                MongoDatabase mongoDatabase = mongoClient.getDatabase("hello");
                /* Add collection */
                MongoCollection<Document> mongoCollection = mongoDatabase.getCollection("user");
                if (mongoCollection == null) {
                    mongoDatabase.createCollection("user");
                    mongoCollection = mongoDatabase.getCollection("user");
                }
                /* Insert document */
                mongoCollection.insertOne(new Document("user", "Alice").append("sex", "F"));
                List<Document> documents = new ArrayList<Document>();
                documents.add(new Document("user", "Bob").append("sex","M"));
                documents.add(new Document("user", "Chris").append("sex","F"));
                mongoCollection.insertMany(documents);
                /* Find documents */
                FindIterable<Document> findIterable = mongoCollection.find();
                MongoCursor<Document> mongoCursor = findIterable.iterator();
                while (mongoCursor.hasNext()) {
                    System.out.println(mongoCursor.next());
                }
                mongoClient.close();
            } catch (Exception e) {
                System.err.println(e.getMessage());
            }
        }
    }

## 4. API接口说明

具体API说明参考链接手册

[http://mongodb.github.io/mongo-java-driver/3.0/driver/getting-started/quick-tour/](http://mongodb.github.io/mongo-java-driver/3.0/driver/getting-started/quick-tour/)