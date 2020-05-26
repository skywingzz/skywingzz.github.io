---
title: Kafka Offset
date: 2020-05-20 15:50:00 +0300
categories: [Kafka]
tags: [Kafka, Offset, Consumer]
---

# Offset 이란?
Partition 의 특정 Consumer 가 메세지를 읽어들인 위치를 나타냄.
Consumer 가 메세지 수신 처리를 재개할 때 어떤 메세지 부터 가져 가야 하는 지 알 수 있음.
![](/assets/images/log_consumer.png)

# Offset Topic
Kafka 에는 Consumer 의 offset 을 저장 하는 topic 이 존재함. (v0.9 release)

* __consumer_offsets
  * 일반 토픽처럼 partitioning, replication 되어 있음
  ```yaml
  offsets.topic.replication.factor=1 (default=1)
  ```
Consumer 가 Kafka에 현재까지 읽은 메세지의 offset 정보를 알려주는 것을 **commit** 이라 한다.

# Offset Option
* enable.auto.commit (default=true)
	* 특정 주기 마다 자동으로 commit 하는 설정
* auto.commit.interval.ms (default=5000ms)
	* 커밋 주기
	* kafka 로 부터 메세지를 읽어 올때 이 주기와 맞으면 offset 정보를 commit 한다.
* enable.auto.commit = false 일 경우 이 옵션은 무시된다.
![](/assets/images/kafka_commit_1.png)
* auto commit 일 경우 장애 발생 타이밍에 따라 commit 된 메세지 처리가 완료 되지 않거나
  * 메세지 처리 누락 발생
* 메세지 처리가 완료 되었지만 offset commit 이 이루어 지지 않은 경우가 발생 할 수 있음 
  * 동일 메세지 중복 처리
![](/assets/images/kafka_commit_2.png)
* auto.offset.reset
  * Consumer 의 offset  commit 정보가 존재 않거나 해당 offset 이 유효하지 않을 경우
    * latest : 가장 새로운(마지막) offset부터 (Default)
    * earliest : 가장 오래된(처음) offset부터
    * none : 해당 consumer group이 가져가고자 하는 topic의 consumer offset정보가 없으면 exception을 발생시킴(seek 명령어로 명시적으로 offset 재지정)
* Spring Kafka Configuration
  ```yaml
  spring:
    kafka:
        consumer:
        auto-offset-reset: earliest
        enable-auto-commit: true
        auto-commit-interval: 5000ms
  ```

# Offset Commit
* Commit 방법
  * commitSync()
    ```java
    while (true) {
        ConsumerRecords<String, String> records = consumer.poll(100);
        for (ConsumerRecord<String, String> record : records)
            System.out.println(record.value());
    
        try {
            consumer.commitSync();
        } catch (CommitFailedException e) {
            System.err.println("commit failed");
        }
    }
    ```
    * 신뢰도가 가장 높음
    * poll 메서드에서 반환된 마지막 오프셋을 커밋
    * 커밋이 성공하거나 재시도가 불가능한 에러가 생길 때까지 commitSync() 를 재시도
    * commitSync() 호출전에 리밸런싱이 시작된다면 중복 처리됨
    * 브로커가 커밋 요청에 응답할 때 까지 기다려야 하기 때문에 Consumer 처리량에 제한이 생김
  * commitAsync()
    ```java
    while (true) {
        ConsumerRecords<String, String> records = consumer.poll(100);
        for (ConsumerRecord<String, String> record : records) 
            System.out.println(record.value());
    
        consumer.commitAsync();
    }
    ```
    * 커밋 요청을 전송하고 처리를 계속 진행
    * callback 전달 가능
        ```java
        while (true) {
            ConsumerRecords<String, String> records = consumer.poll(100);
            for (ConsumerRecord<String, String> record : records)
                System.out.printf(record.value());
            
            consumer.commitAsync(new OffsetCommitCallback() {
                public void onComplete(Map<TopicPartition, OffsetAndMetadata> offsets, Exception e) {
                    if (e != null)
                        System.err.println("Commit failed");
                    else
                        System.out.println("Commit succeeded");
                    if (e != null)
                        log.error("Commit failed for offsets {}", offsets, e);
                }
            });
        }
        ```
* Spring Kafka AckMode
    * enable-auto-commit=true 일 경우 이옵션은 무시된다.
    * 모두 commitAsync() 을 실행 시킨다.
    ```yaml
    spring:
        kafka:
            listener:
                ack-mode: RECORD
                ack-time: 5000ms
                ack-count: 100
    ```
    * RECORD
      * 레코드가 처리 될 때 마다 실행
    * BATCH (Default)
      * 다음 poll 한 레코드가 모두 수행되고 다음 poll 이 수행되기 전에 배치로 실행
    * TIME
      * akc-time 설정 시간 단위 
    * COUNT
      * 설정된 처리 횟수 후
    * COUNT_TIME
      * ack-time 또는 ack-count 둘중 하나가 만족될때
    * MANUAL
      * ack 가 수동으로 발생했을 경우. 하지만 뒤에서 배치로 실행
    * MANUAL_IMMEDIATE
      * Acknowledgment.acknowledge() 가 실행될 때 즉시 처리
```java
@Component
public class KafkaConsumer {
    private static final Logger LOGGER = LoggerFactory.getLogger(KafkaConsumer.class);

    @Override
    @KafkaListener(topics = "product.event", groupId = "finding-consumer-group")
    public void messageListener(ConsumerRecord<String, String> consumerRecord, Acknowledgment acknowledgment) {
        LOGGER.info("Received Message Consumer : " + consumerRecord);
        acknowledgment.acknowledge();
    }
}
```

# 리밸런싱 & 특정 offset 부터 읽기
* ConsumerRebalanceListener interface 구현
  * onPartitionsRevoked(Collection<TopicPartition> partitions)
    * 리밸런싱이 시작되기 전, 그리고 컨슈머가 메세지 소비를 중단한 후 호출
    * offset 를 커밋 해야 하는 곳 (저장)
  * onPartitionsAssigned(Collection<TopicPartition> partitions)
    * 파티션이 브로커에게 재할당된 후, 그리고 컨슈머가 파티션을 새로 할당 받아 메세지 소비를 시작하기 전에 호출
    * consumer.seek() 를 이용하여 장애 발생 offset 부터 읽음
* consumer.subscriber(topics, Listener구현체)
    ``` java
    public class SaveOffsetsOnRebalance implements ConsumerRebalanceListener {
        private Consumer<?,?> consumer;

        public SaveOffsetsOnRebalance(Consumer<?,?> consumer) {
            this.consumer = consumer;
        }

        public void onPartitionsRevoked(Collection<TopicPartition> partitions) {
            // save the offsets in an external store using some custom code not described here
            for(TopicPartition partition: partitions)
                saveOffsetInExternalStore(consumer.position(partition));
        }

        public void onPartitionsAssigned(Collection<TopicPartition> partitions) {
            // read the offsets from an external store using some custom code not described here
            for(TopicPartition partition: partitions)
                consumer.seek(partition, readOffsetFromExternalStore(partition));
        }
    }


    // Consumer
    consumer.subscribe(topics, SaveOffsetsOnRebalance);
    ```
* spring kafka
  * Rebalancing Listener : https://docs.spring.io/spring-kafka/reference/html/#rebalance-listeners
  * Seek : https://docs.spring.io/spring-kafka/reference/html/#seek

# 참고
* Spark Offset Management
![](/assets/images/Spark-Streaming-flow-for-offsets.png)