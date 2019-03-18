# fx-orders-capture - Overview

This is part of a data service project that uses java spring microservice (fx-orders-capture), kafka, cassandra to capture a hypothetical forex trade and publishes it to kafka topic (tradeCapture) and awaits for an acknowledgement for upto 5 seconds. There is another microservice (fx-orders-processor) that process the trade and inserts into a cassandra table (orders) under keyspace traderecords. 


We also have a search operation where an user can search a trade by using the order ID. As soon an user enters order ID in the search area and press enter the microservice (fx-orders-capture) publishes that order to kafka topic and awaits for a response upto 5 seconds before a time out response is returned back to front end. The other microservice (fx-orders-processor) retrieves that search ID from the request topic (tradeRequest) and retrieves the trade from cassandra table (orders) and publishes that trade to response topic (tradeReply). The requesting microdervice (fx-orders-capture) than picks that response and returns back to front end and display it.

We would be using spring boot thymeleaf for the front-end part to make this project simpler but it may have been better to use angular of react as well. 



# This microservice (fx-orders-capture) is responsible for 

* to capture an order and publish in to kafka topic (tradeCapture) and get an acknowledgement
* to get a order ID to publish it to kafka topic (tradeRequest) 
* retrive a response from kafka topic (tradeReply) and display it to front end

## Overall architecture
![architecture](doc/architecture.jpg)


## Basic configuration

This sample application shows how 

## Start Zookeeper
* Download zookeeper from http://zookeeper.apache.org/releases.html#download and extract it to local drive. 
* Rename “zoo_sample.cfg” to “zoo.cfg” (for windows machines only). 
* Also edit dataDir entry inside zoo.cfg to a valid path e.g. \zookeeper\data (for windows machines only).
* Ensure you have java installed and JAVA_HOME is set in the system environment variable (for windows machines only). 
* Add ZOOKEEPER_HOME environment variable upto bin path of your zookeeper directory (for windows machines only)
* Add ;%ZOOKEEPER_HOME%\bin; to PATH variable (for windows machines only)

- `double click bin/zkServer.cmd` (windows)
- `bin/zookeeper-server-start.sh config/zookeeper.properties` (UNIX)


## Start Kafka Server
* Download kafka from https://kafka.apache.org/
* Extract it and edit “server.properties” inside config folder
* Change log.dirs to a valid path e.g. (windows) “=C:\\kafka\\kafka-logs”
* Open a dos shell and go inside kafka bin directory e.g. cd C:\kafka\kafka_2.11-1.0.0\bin\windows

- `kafka-server-start.bat ../../config/server.properties` (UNIX)
- `bin/kafka-server-start.sh config/server.properties` (UNIX)

## Create Kafka Topic
- `kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic tradeCapture` (UNIX)

- `kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic tradeRequest` (UNIX)

- `kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic tradeReply` (UNIX)

- `kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic tradeCapture` (Windows)

- `kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic tradeRequest` (Windows)

- `kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic tradeReply` (Windows)


## Start Cassandra
* dwonload cassandra from http://cassandra.apache.org/download/
* Extract it to local drive
* Simply run the installer and leave everything default
* Go to cassandra installation directory inside bin folder
* Leave everything default

- `double click on cassandra.bat` (Windows)
- `run equivalent shell script in unix` (UNIX)

## By default cassandra will run on port 9042 and Kafka will run on port 9092 and Apache Zookeeper will run on port 2181. If you change any of these default setting then accordingly update microservice properties inside resource folder.

JUnit Test
------------------

If you have git-bash installed and configured in the path then use below command to checkout source code into your system,

git clone https://github.com/pmjobsearch01/fx-orders-capture.git

If you have maven installed and in the path then use below command to build and run junit test cases that proves the core functions,

- Go to the folder where the code is checked-out (you should see the pom.xml in your current folder)

cd fx-orders-capture

mvn clean install

Run Application
--------------------------
mvn spring-boot:run

Test from Browser (GUI)
-------------------------
Once application is up and running by using above run command then open a browser and test the application using below url

http://localhost:8080/



![newOrder](doc/newOrder.jpg)

![searchOrder](doc/searchOrder.jpg)
