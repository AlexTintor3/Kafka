	-Iniciar Zookeper: bin/zookeeper-server-start.sh config/zookeeper.properties

	-Iniciar Kafka: bin/kafka-server-start.sh config/server.properties

	-Creación de topics: bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test

	-Listar topics: bin/kafka-topics.sh --list --zookeeper localhost:2181

	-Consumidor: bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test
	-Productor: bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
--------------------------------------------------------------------------------------------------------------------------------------------
CLUSTER MULTIBROKER

	- Kafka-1: cp config/server.properties config/server-1.properties

broker.id=1
listeners=PLAINTEXT://:9093
log.dirs=/usr/local/var/lib/kafka-logs-1

	- Kafka-2: cp config/server.properties config/server-2.properties

broker.id=2
listeners=PLAINTEXT://:9094
log.dirs=/usr/local/var/lib/kafka-logs-2

- Iniciar servicios:

		kafka-server-start config/server.properties
		kafka-server-start config/server-1.properties
		kafka-server-start config/server-2.properties

- Crear tema replicado:

		kafka-topics --zookeeper localhost:2181 --create --replication-factor 3 --partitions 1 --topic replicated-topic
		kafka-topics --zookeeper localhost:2181 --describe --topic replicated-topic
---------------------------------------------------------------------------------------------------------------------------------------------

-- Prueba de fallos --

	kafka-console-producer --broker-list localhost:9092 --topic replicated-topic

	- Matar a Kafka-1: ps aux | grep server-1.properties


Conectar con jar Kafka y Spark:

	pyspark --jars spark-streaming-kafka-0-8-assembly_2.11-2.4.1.jar

  	pyspark --packages org.apache.spark:spark-streaming-kafka-0-8_2.11:2.2.0
