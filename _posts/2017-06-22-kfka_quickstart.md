---
layout: post
title:  "Kfka Quickstart"
date:   2017-06-13 08:43:59
author: Beom
categories: Big_data
---
1. 다운로드
https://kafka.apache.org/downloads 에서 kafka를 다운받는다.

2. 옵션 설정
주키퍼 config
vi config/zookeeper.properties
카프카 config
vi config/server.properties

3. 서버 스타트
주키퍼 시작
 bin/zookeeper-server-start.sh config/zookeeper.properties

 카프카 시작
 bin/kafka-server-start.sh config/server.properties
 
 데몬 형태로 시작 할 경우에는 -daemon 옵션을 준다.
 bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
 bin/kafka-server-start.sh -daemon config/server.properties
 
4. 토픽만들기 
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
데모용임으로 복제와 파티션 옵션은 1로 설정한다. zookeeper 옵션에서는 zookeeper가 설치된 IP 또는 로스트명을 입력한다. (localhost 실행전에 hostname을 꼭 확인 한다.)

5. Producer/Consumer 테스트
```
$ bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
  Hello world
```
프로듀서를 실행 시키고 Hello world 메세지를 입력 한다.
```
> bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
Hello world
```
컨슈머를 실행 시키면 프로듀서에서 입력한 메세지가 출력 되는 것을 볼 수 있다.