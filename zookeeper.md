[TOC]

## 开启zookeeper的metric
监控zookeeper使用jvm_exporter来采集数据，jvm_exporter是一个可以配置抓取和暴露JMX目标的mBeans的收集器

/data/zookeeper/jmx_exporter/zookeeper.yml
```YAML
rules:
  # replicated Zookeeper
  - pattern: "org.apache.ZooKeeperService<name0=ReplicatedServer_id(\\d+)><>(\\w+)"
    name: "zookeeper_$2"
  - pattern: "org.apache.ZooKeeperService<name0=ReplicatedServer_id(\\d+), name1=replica.(\\d+)><>(\\w+)"
    name: "zookeeper_$3"
    labels:
      replicaId: "$2"
  - pattern: "org.apache.ZooKeeperService<name0=ReplicatedServer_id(\\d+), name1=replica.(\\d+), name2=(\\w+)><>(\\w+)"
    name: "zookeeper_$4"
    labels:
      replicaId: "$2"
      memberType: "$3"
  - pattern: "org.apache.ZooKeeperService<name0=ReplicatedServer_id(\\d+), name1=replica.(\\d+), name2=(\\w+), name3=(\\w+)><>(\\w+)"
    name: "zookeeper_$4_$5"
    labels:
      replicaId: "$2"
      memberType: "$3"
  # standalone Zookeeper
  - pattern: "org.apache.ZooKeeperService<name0=StandaloneServer_port(\\d+)><>(\\w+)"
    name: "zookeeper_$2"
  - pattern: "org.apache.ZooKeeperService<name0=StandaloneServer_port(\\d+), name1=InMemoryDataTree><>(\\w+)"
    name: "zookeeper_$2"
```
bin/zkServer.sh
```shell
## 新增javaagent
JMX_DIR="/data/zookeeper/jmx_exporter"
JVMFLAGS="$JVMFLAGS -javaagent:$JMX_DIR/jmx_prometheus_javaagent-0.3.1.jar=9505:$JMX_DIR/zookeeper.yml"
```

[jmx_exporter](https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.16.0/jmx_prometheus_javaagent-0.16.0.jar)
[zookeeper优化](https://blog.csdn.net/qq_31547771/article/details/105813318)
[zk参数](https://blog.csdn.net/weixin_38424594/article/details/112986217)
