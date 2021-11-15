[TOC]

# kafka操作指令
## topic
```shell
list   topic: bin/kafka-topics.sh --bootstrap-server broker_host:port --list
create topic: bin/kafka-topics.sh --bootstrap-server broker_host:port --create --topic my_topic_name --partitions 2 --replication-factor 3 --config x=y
modify topic: bin/kafka-topics.sh --bootstrap-server broker_host:port --alter --topic my_topic_name --partitions 4
delete topic: bin/kafka-topics.sh --bootstrap-server broker_host:port --delete --topic my_topic_name
m/add config: bin/kafka-configs.sh --bootstrap-server broker_host:port --entity-type topics --entity-name my_topic_name --alter --add-config x=y
m/del config: bin/kafka-configs.sh --bootstrap-server broker_host:port --entity-type topics --entity-name my_topic_name --alter --delete-config x
blance kafka: bin/kafka-preferred-replica-election.sh --bootstrap-server broker_host:port
```
## consumer-groups
```shell
kafka-consumer-groups.sh --bootstrap-server broker_host:port --list     //查看consumer-groups列表
kafka-consumer-groups.sh --bootstrap-server broker_host:port --describe --group my_group_name --[members|verbose|offsets|state]               //查看consumer详情
kafka-consumer-groups.sh --bootstrap-server broker_host:port --group my_group_name --topic [my_topic_name|--all-topics] --reset-offsets [--to-earliest|--to-offset my_offset_num|--shift-by +-my_offset_num] –execute     //把某group位于某topic上的offset移动到指定offset,如最前面，绝对值offset，相对当前offset
```
## console-producer
```shell
kafka-console-producer.sh --broker-list broker_host:port --topic my_topic_name      //创建生产者
```
## console-consumer
```shell
kafka-console-consumer.sh --bootstrap-server broker_host:port --topic my_topic_name     //创建消费者
kafka-console-consumer.sh --bootstrap-server broker_host:port --topic my_topic_name --from-beginning        //查看topic消费记录
```
## run-class
```shell
kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list broker_host:port --topic my_topic_name --time -1      //查看topic各partition的offset最小最大值
```
## kafka运行参数
受控leader迁移时sync日志：controlled.shutdown.enable=true
自动平衡boker： auto.leader.rebalance.enable=true

## ERR
### err: INVALID_FETCH_SESSION_EPOCH
所用的sdk版本或者client高于broker
### 查看版本号
ps -ef|grep '/libs/kafka.\{2,40\}.jar'
find ./libs/ -name \*kafka_\* | head -1 | grep -o '\kafka[^\n]*'
返回的文件名，前面是scala版本号，后面是kafka的版本号.

kafka监控指标
```
|---|---|
|kafka_topic_partition_current_offset |topic的偏移量
|sum(rate(kafka_topic_partition_current_offset{instance="$instance", topic=~"$topic"}[1m])) by (topic) | 每秒钟消息数
|sum(delta(kafka_topic_partition_current_offset{instance=~"$instance", topic=~"$topic"}[5m])/5) by (topic) | 每分钟消息数
|kafka_consumergroup_current_offset | 消费组的偏移量
|sum(delta(kafka_consumergroup_current_offset{instance=~"$instance",topic=~"$topic"}[5m])/5) by (consumergroup, topic) | 每分钟消费组消费topic消息数
|kafka_consumergroup_lag | kafka消费组发生的滞后数
|sum(kafka_consumergroup_lag{instance="$instance",topic=~"$topic"}) by (consumergroup, topic) | 根据消费组和topic查找滞后数
|kafka_topic_partitions | topic分区数
```
[kafka](https://www.cnblogs.com/kevingrace/p/9443270.html)
[消息队列数据积压](https://blog.51cto.com/u_12473494/2420105)
[kafka-exporter](https://cloud.tencent.com/developer/article/1794971)
[kafka lag](https://blog.csdn.net/u013256816/article/details/79955578)
