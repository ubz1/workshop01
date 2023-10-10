# workshop01

### Your mission:

Have 2 subscribers pick up an insert:
db.source.insertOne({success:1})

![](topic-subscribers-get-mongodb-updates-example.png)

### Here is how:

#### Start a replica set or have one on Atlas:

mongod --dbpath dbpath --bind_ip 0.0.0.0 --port 40000 --replSet rs

#### Start Kafka (standalone):

- If you don't have JRE: Download Java: https://www.java.com/en/download/manual.jsp 
- Download Kafka: https://kafka.apache.org/downloads (binary .tgz file)
- Download newest MongoDB connector JAR file (pick the "all" version): https://repo1.maven.org/maven2/org/mongodb/kafka/mongo-kafka-connect/1.9.1/ 
- Extract it into a directory, it's your "plugin.path"

```
tar xfz kafka_2.13-3.5.1.tgz
cd kafka_2.13-3.5.1
Edit: config/connect-standalone.properties , add plugin.path=
./bin/zookeeper-server-start.sh ./config/zookeeper.properties
./bin/kafka-server-start.sh ./config/server.properties
```

#### Create your first topic:

```
./bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic test.source --replication-factor 1 --partitions 1
```

#### Subscribe to your first topic:

```
bin/kafka-console-consumer.sh --topic test.source --bootstrap-server localhost:9092
```

#### Start Kafka Connector with MongoDB Connector:

Create your MongoSourceConnector.properties , example: https://github.com/mongodb/mongo-kafka/blob/master/config/MongoSourceConnector.properties 
and start the connector:

```
./bin/connect-standalone.sh ./config/connect-standalone.properties ./MongoSourceConnector.properties
```

#### Does it work?

Connect to your database and run:

```
db.source.insertOne({success:1})
```
