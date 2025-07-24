Step 1: Install Java (Kafka requires Java)

sudo apt update
sudo apt install openjdk-11-jdk -y
java -version


Step 2: Download and Extract Apache Kafka
1.Go to the Kafka downloads page and get the latest version URL, or run:

export KAFKA_HEAP_OPTS="-Xmx512M -Xms512M"
curl -O $(curl -s https://www.apache.org/dyn/closer.cgi?path=/kafka/3.5.1/kafka_2.13-3.5.1.tgz | grep -o 'http[^"]*kafka_2.13-3.5.1.tgz')
wget https://downloads.apache.org/kafka/3.5.1/kafka_2.13-3.5.1.tgz

2.Extract it:

tar -xzf kafka_2.13-3.5.1.tgz
cd kafka_2.13-3.5.1


Step 3: Start Zookeeper and Kafka Broker

Kafka uses Zookeeper to coordinate brokers.

Start Zookeeper (default config):

bin/zookeeper-server-start.sh config/zookeeper.properties

Open a new terminal or use a screen/tmux session and start Kafka broker:

bin/kafka-server-start.sh config/server.properties

Now Kafka broker is running!

Step 4: Create a Kafka Topic

Create a topic named test-topic:

bin/kafka-topics.sh --create --topic test-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1


Check if topic is created:

bin/kafka-topics.sh --list --bootstrap-server localhost:9092


Step 5: Create a Simple Producer and Consumer
Producer (send messages):

bin/kafka-console-producer.sh --topic test-topic --bootstrap-server localhost:9092

Type any message and hit Enter to send.


Consumer (read messages):
Open a new terminal and run:

bin/kafka-console-consumer.sh --topic test-topic --from-beginning --bootstrap-server localhost:9092


Validate with small script (Python example)

1. Install Kafka Python client:

pip install kafka-python


2.Simple producer script (producer.py):

from kafka import KafkaProducer

producer = KafkaProducer(bootstrap_servers='localhost:9092')
producer.send('test-topic', b'Hello, Kafka!')
producer.flush()
print("Message sent")

3.Simple consumer script (consumer.py):

from kafka import KafkaConsumer

consumer = KafkaConsumer('test-topic', bootstrap_servers='localhost:9092', auto_offset_reset='earliest', consumer_timeout_ms=1000)
for message in consumer:
    print(f"Received: {message.value.decode('utf-8')}")

Run consumer.py first, then producer.py to see the message flow.
