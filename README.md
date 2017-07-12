  # KafkaSpark
  Simulating Real time data pipeline using Kafka, Spark.

 

Steps to create setup:

1. Create a Scala development environment in IntelliJ, install SBT plugin
2. Create a new Scala SBT project or import already existing Scala SBT project "kafkaexample" in the tar file .
3. Resolve the Scala, spark, Kafka dependencies in build.sbt file, refer to sample build.sbt file in the package
4. After resolving the dependencies, you should be able to compile/build the spark streaming application (KafkaWordCount.scala)

Kafka setup:

1. Download Kafka (any version say 2.11-0.10.2.0), The same version is also zipped in the package
2. Go to the Kafka Folder kafka_2.11-0.10.2.0 for running zookeeper server, kafka server, and kafka producer/consumer console, for more information refer to https://kafka.apache.org/quickstart
3. Configure multiple brokers: (Step 3 is Already done in the attached tar package)
	a. copy the file config\server.properties to another file config\server-1.properties
	b. edit the new file (server-1.properties) and add/update the following lines:
		
		broker.id=1
		listeners=PLAINTEXT://:9093
		log.dir=/tmp/kafka-logs-1

4. Start zookeeper serverusing following command in the command prompt:
	bin\windows\zookeeper-server-start.bat config\zookeeper.properties
5. Start kafka server in another command prompt window:
	bin\windows\kafka-server-start.bat config\server.properties
6. Create a Kafka topic say named "test" for input streaming:
	bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
7. Create a Kafka topic say name "testout" for output streaming from spark:
	bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic testout
8. Start the two new nodes in command window for input topic, output topic:

	bin\windows\kafka-server-start.bat config\server.properties
	bin\windows\kafka-server-start.bat config\server-1.properties

9. 	Execute the following in command window for running spark producer with input topic "test":
	bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic test
	
10. Execute the following in command window for running spark consumer with output topic "testout"
	bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9093 --topic testout


Now run the spark application in IntelliJ and specify the following command line arguments:
	localhost test-consumer-group test 1

After above steps for Kafka Setup, you can send the streaming data from kafka producer console window to spark created in step 9 and observe the ouput in kafka consumer console window created in step 10. In sample applicaiton the word count will be printed on the Kafka Consumer console.
