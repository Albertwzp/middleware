[TOC]
# 配置
```
# 本 broker 所属的 Cluser 名称
brokerClusterName = rocketmq01
# broker 的名称
brokerName=broker-a
# 接受客户端连接的监听端口
listenPort=30911
# NameServer 集群地址
namesrvAddr=:9876;:9876
# 存储数据的根路径
storePathRootDir=/data/rocketmq/store
# commitLog存储路径
storePathCommitLog=/data/rocketmq/store/commitlog
# 消费队列存储路径
storePathConsumeQueue=/data/rocketmq/store/consumequeue
# 消费队列存储路径
storePathIndex=/data/rocketmq/store/index

# Broker 角色分为 ASYNC_MASTER（异步主机）、SYNC_MASTER（同步主机）以及SLAVE（从机）
brokerRole=SYNC_MASTER
# 消息刷盘模式 SYNC_FLUSH/ASYNC_FLUSH，SYNC_FLUSH 模式下的 broker 保证在收到确认生产者之前将消息刷盘。ASYNC_FLUSH 模式下的 broker 则利用刷盘一组消息的模式，可以取得更好的性能。
flushDiskType=ASYNC_FLUSH
# 在每天的什么时间删除已经超过文件保留时间的 commit log
deleteWhen=04
# 以小时计算的文件保留时间
fileReservedTime=168

# 是否启动 DLedger
enableDLegerCommitLog=true
# DLedger Raft Group的名字，建议和 brokerName 保持一致
dLegerGroup=broker-a
# DLedger Group 内各节点的端口信息，同一个 Group 内的各个节点配置必须要保证一致
dLegerPeers=n0-172.19.67.164:40911;n1-172.19.67.165:40911;n2-172.19.67.166:40911
## must be unique，节点 id, 必须属于 dLegerPeers 中的一个；同 Group 内各个节点要唯一
dLegerSelfId=n0
# 发送线程个数，建议配置成 Cpu 核数；同步刷盘建议适当增大sendMessageThreadPoolNums，具体配置需要经过压测。
sendMessageThreadPoolNums=8

# 是否允许 Broker 自动创建Topic，建议线下开启，线上关闭，默认为 true
autoCreateTopicEnable=false
# 是否允许 Broker 自动创建订阅组，建议线下开启，线上关闭，默认为 true
autoCreateSubscriptionGroup=false
# 异步刷盘建议使用自旋锁，同步刷盘建议使用重入锁，调整Broker配置项useReentrantLockWhenPutMessage，默认为false，表示使用自旋锁，为true，表示使用重入锁；
# useReentrantLockWhenPutMessage=false
# 发送消息线程等待时间，默认200ms
# waitTimeMillsInSendQueue=1000
# 开启临时存储池，消息写入到堆外内存，消费时从pageCache消费，读写分离，提升集群性能；异步刷盘建议开启TransientStorePoolEnable；
transientStorePoolEnable=true
# 开启Slave读权限（分担master 压力），消息占用物理内存的大小通过accessMessageInMemoryMaxRatio来配置默认为40%；如果消费的消息不在内存中，开启slaveReadEnable时会从slave节点读取；提高Master内存利用率
slaveReadEnable=true
# 关闭堆内存数据传输，Broker响应消费请求时，不必将数据重新读到堆内存再发送给客户端；直接从PageCache将数据发送给客户端；建议关闭transferMsgByHeap，提高拉消息效率；
transferMsgByHeap=false
# 开启文件预热，避免日志文件在分配内存时缺页中断
warmMapedFileEnable=true
```
# RocketMQ Command
listip topic: bin/mqadmin allocateMQ -n localhost:9876 -t TOPIC -i ipList
delete topic: bin/mqadmin deleteTopic -n localhost:9876 -t TOPIC -c DefultCluster
list cluster: bin/mqadmin topicClusterList -n localhost:9876 -t SCANRECORD
list   topic: bin/mqadmin topicList -n localhost:9876
get    route: bin/mqadmin topicRoute -n localhost:9876 -t SCANRECORD
status topic: bin/mqadmin topicStatus -n localhost:9876 -t SCANRECORD
modify  priv: bin/mqadmin updateTopicPerm -t SCANRECORD -p put -n localhost:9876
add    topic: bin/mqadmin updateTopic -c DefaultCluster -n localhost:9876 -t TOPIC -r 12 -w 12
update  conf: bin/mqadmin [get|update]BrokerConfig -c [$Cluster] -k fileReservedTime -v 72
              bin/mqadmin [get|update]BrokerConfig -b [$IP]:10911 -k fileReservedTime -v 72

[rocketmq 优化](https://www.jianshu.com/p/28f23733a53d)
[调优](https://cloud.tencent.com/developer/article/1496414)
