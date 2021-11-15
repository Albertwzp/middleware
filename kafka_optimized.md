# kafka调优

## 内存
```SHELL
# JVM默认启动堆内存1G，生产环境不推荐超过6G,使用G1代替CMS垃圾回收
-Xms6g –Xmx6g 
-XX:PermSize=48m 
-XX:MaxPermSize=48m 
-XX:+UseG1GC-XX:MaxGCPauseMillis=20 
-XX:InitiatingHeapOccupancyPercent=35
```
## CPU
```
Kafka不属于计算密集型（CPU-bound）的系统，应该追求多核而非高时钟频率，CPU核数最好大于8
kafka会使用大量文件和网络socket，所以，我们需要把file descriptors的默认配置改为100000
```

## producer.properties
```INI
# 默认发送不进行压缩，推荐配置一种适合的压缩算法，可以大幅度的减缓网络压力和Broker的存储压力
compression.type:none 
# 在Producer端用来存放尚未发送出去的Message的缓冲区大小,缓冲区满了之后可以选择阻塞发送或抛出异常，由block.on.buffer.full的配置来决定
buffer.memory:33554432
```

## consumer.preoperties
```
#启动Consumer的个数，适当增加可以提高并发度
num.consumer.fetchers:1
#每次Fetch Request至少要拿到多少字节的数据才可以返回。
fetch.min.bytes:1
#在Fetch Request获取的数据至少达到fetch.min.bytes之前，允许等待的最大时长。对应上面说到的Purgatory中请求的超时时间
fetch.wait.max.ms:100
```

## server.propertie
```INI
# 优化处理消息的最大线程数 默认是3 可以调成CPU数+1
num.network.threads=
# broker处理磁盘IO线程数 CUP数*2
num.io.threads=
# 调整log的文件刷盘策略 10000条或者1秒
log.flush.interval.messages=10000
log.flush.interval.ms=1000
# num.recovery.threads.per.data.dir=1
# 日志保留策略配置
log.retention.hours=72
log.cleaner.delete.retention.ms //个别topic设置
# 默认的Replica数量
offsets.topic.replication.factor=3
# 这个参数表示单条消息的最大长度
message.max.bytes=(单位byte，默认1M)
# broker可复制的消息的最大字节数。这个值应该比message.max.bytes大，否则broker会接收此消息，但无法将此消息复制出去，从而造成数据丢失
replica.fetch.max.bytes=(单位byte，默认1M)
# 消费者能读取的最大消息,这个值应该大于或等于message.max.bytes,否则会导致消费者无法消费到消息，并且不会爆出异常或者警告，导致消息在broker中累积
fetch.message.max.bytes=
```

# example
```YAML
broker.id=1

#
listeners=PLAINTEXT://172.25.32.62:9092

# cpu
num.network.threads=8

# cpu×2
num.io.threads=16

socket.send.buffer.bytes=102400

socket.receive.buffer.bytes=102400

socket.request.max.bytes=209715200

# 多盘读写，避免切换调度
log.dirs=/data/kafka/data/kafkadata1,/data/kafka/data/kafkadata2,/data/kafka/data/kafkadata3,/data/kafka/data/kafkadata4
num.partitions=1

num.recovery.threads.per.data.dir=1

offsets.topic.replication.factor=3
transaction.state.log.replication.factor=3
transaction.state.log.min.isr=2

log.retention.hours=168

log.segment.bytes=1073741824

log.retention.check.interval.ms=300000

# 
zookeeper.connect=172.25.32.62:2181,172.25.32.63:2181,172.25.32.64:2181,172.25.32.65:2181,172.25.32.66:2181/kafka

zookeeper.connection.timeout.ms=100000

group.initial.rebalance.delay.ms=3000

#
auto.create.topics.enable=true

delete.topic.enable=true

message.max.bytes=31457280
replica.fetch.max.bytes=31457280

default.replication.factor=3
```

[kafka优化](https://blog.csdn.net/bluetjs/article/details/80485359)
[kafka压缩](https://blog.csdn.net/zhang18024666607/article/details/113118470)
[kafka参数](https://www.cnblogs.com/successok/p/14218953.html)
