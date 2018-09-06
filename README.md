# WikiAnalysis
It takes datastream from Wikipedia and count the number of bytes edited in last 5 seconds using flink and push the result into kafka.

The overall structure is the following:
##### SOURCE_STREAM => PROCESSING_ENGINE => SINK_RESULT 

  - Source_stream :: Stream from Wikipedia through Wiki connector to flink
  - Processing_engine :: Flink
  - Sink_result :: Kafka producer

## Prerequisite
  - kafka 2.0
  - flink 1.6
## Setup instructions

Go to Kafka installed directory and write the following commands. 
```sh
$ ./bin/zookeeper-server-start.sh config/zookeeper.properties
```
```sh
$ ./bin/kafka-server-start.sh config/server.properties
```
```sh
$ ./bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic wiki-results
```
```sh
$ ./bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic wiki-results --from-beginning
```
Go to Flink installed directory and write the following commands. 

```sh
$ ./bin/start-cluster.sh
```

Go the repo directory and type the following commands.
```sh
$  mvn clean package
```
```sh
$   mvn exec:java -Dexec.mainClass=wikiedits.WikipediaAnalysis
```

The Second command will run the jar file and you start getting some result in consumer terminal of kafka but it wont register into flink web portal.

To run it on flink:
- Go to your favorite browser and enter this url http://localhost:8081 
- Go to Submit New Job Tab. 
- Click on add new job.
- Select the path/to/your/clone_dir/target/wiki-edits-0.1.jar
- Click upload
- Checkbox the newly submitted job 
- In the class inputbox type ```wikiedits.WikipediaAnalysis```

You can now analyse the Job in the web portal and can check consumer terminal of kafka to see the results.

Note : The commands above is for linux based system, for using in windows use .bat file instead of .sh file
