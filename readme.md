# Step1:
Run setup:
/usr/bin/zookeeper-server-start zookeeper.properties
/usr/bin/kafka-server-start producer.properties

# Step2:
Produce data in "calls" topic:
python kafka_server.py

# Step3:
Check the kafka topic and consume data:
/usr/bin/kafka-topics --describe --zookeeper localhost:2181 --topic calls
kafka-console-consumer --bootstrap-server localhost:9092 --from-beginning --topic calls

# Step4:
Run Spark Streaming:
spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.3.4 --master local[*] data_stream.py



## Question 1: How did changing values on the SparkSession property parameters affect the throughput and latency of the data?
We can tweak the number of micro-batches received with processRowsPerSecond using maxOffsetPerTrigger and maxRatePerPartition options.
maxRatePerPartition: sets the maximum number of messages received per batch
maxOffsetPerTrigger: limits the number of records to fetch per trigger

## Question 2: What were the 2-3 most efficient SparkSession property key/value pairs? Through testing multiple variations on values, how can you tell these were the most optimal?
The idea is to increase processRowsPerSecond.
spark.default.parallelism: total number of cores on all executor nodes.
spark.sql.shuffle.partitions: number of partitions used for data shuffles
spark.streaming.kafka.maxRatePerPartition: maximum rate at which data will be read from each Kafka partition.
