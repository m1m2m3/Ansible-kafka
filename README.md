Ansible-kafka
=========

A brief description of the role goes here.

Requirements
------------

Kafka and Zookeeper package file

Role Variables
--------------

As per this program we used only one variable file /vars/test_linux.yml for configure zookeeper.service and kafka.service

Dependencies
------------

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters):

<br>---</br>
<br> hosts: localhost</br>
<br> gather_facts: no</br>
<br> tasks:</br>
<br>   - name: Call ansible kafka file</br>
<br>     include_role:</br>
<br>       name: Ansible-kafka</br>
<br>     vars:</br>
<br>       os: linux</br>
<br>       env: test</br>
<br>       tags: always</br>

<h3> To run the playbook </h3>
 ansible-playbook playbook.yml -e "os=linux tags_list=install"

Detailed info
-------
<h3>What is Kafka</h3>
Apache Kafka is an open-source stream-processing software platform developed by LinkedIn and donated to the Apache Software Foundation, written in Scala and Java. The project aims to provide a unified, high-throughput, low-latency platform for handling real-time data feeds

<h3>What is Zookeeper</h3>
Apache ZooKeeper is a software project of the Apache Software Foundation. It is essentially a service for distributed systems offering a hierarchical key-value store, which is used to provide a distributed configuration service, synchronization service, and naming registry for large distributed systems.

<h3>other in general </h3>
 *Create the unit file /etc/systemd/system/zookeeper.service with the following content

 *we don't need to write the version number three times because of the symlink we created. The same applies to the next unit file for Kafka, /etc/systemd/system/kafka.service
 
 *To test functionality, we'll connect to Kafka with one producer and one consumer client. The messages provided by the producer should appear on the console of the consumer. But before this we need a medium these two exchange messages on. We create a new channel of data called topic in Kafka's terms, where the provider will publish, and where the consumer will subscribe to. We'll call the topic Opstree. We'll use the kafka user to create the topic:
 
 To test functionality, we'll connect to Kafka with one producer and one consumer client. The messages provided by the producer should appear on the console of the consumer. But before this we need a medium these two exchange messages on. We create a new channel of data called topic in Kafka's terms, where the provider will publish, and where the consumer will subscribe to. We'll call the topic Opstree. We'll use the kafka user to create the topic:
$ /opt/kafka/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic Opstree
 

We start a consumer client from the command line that will subscribe to the (at this point empty) topic created in the previous step:
$ /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic Opstree --from-beginning
We leave the console and the client running in it open. This console is where we will receive the message we publish with the producer client.
On another terminal, we start a producer client, and publish some messages to the topic we created. We can query Kafka for available topics:
$ /opt/kafka/bin/kafka-topics.sh --list --zookeeper localhost:2181
Opstree
And connect to the one the consumer is subscribed, then send a message:
$ /opt/kafka/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic Opstree
> Welcome to Opstree #2

At the consumer terminal, the message should appear shortly:
$ /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic FirstKafkaTopic --from-beginning
 Welcome to Opstree #2

Author Information
------------------

Akshay Namdev
