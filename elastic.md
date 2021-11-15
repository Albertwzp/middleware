
## es信息查询
```
export AUTH=$(echo -n 'user:pass'|base64)
export SERVER=D.D.D.D:9200

AUTH=$(echo -n 'index:Z7yTouAmVCGVtuOpIxXC'|base64)
USR='index:Z7yTouAmVCGVtuOpIxXC'
#SERVER=S.S.S.S:9200
#SERVER=D.D.D.D:9200
#DST=D.D.D.D:9200
#curl -X GET -H "Authorization: Basic $AUTH" http://${SERVER}/_recovery?pretty >_recovery.json
# 未分配原因
curl -s -X GET -H "Authorization: Basic $AUTH" http://${SERVER}/_cluster/allocation/explain?pretty >explain.json
egrep '"name"|file_descriptors"' node_stats.json
#curl -s -XGET -H "Authorization: Basic $AUTH" http://${SERVER}/_cat/shards?h=index,shard,prirep,state,unassigned.reason >shards.txt
#curl -s -XGET -H "Authorization: Basic $AUTH" http://${SERVER}/_cat/shards?v >shards.txt

#curl -s -XGET -H "Authorization: Basic $AUTH" http://${SERVER}/_nodes/stats?pretty=true >node_stats.json
#索引查询
#curl -s -XGET -H "Authorization: Basic $AUTH" http://${SERVER}/bar/_mapping?pretty >bar.json
#curl -s -XGET -H "Authorization: Basic $AUTH" http://${SERVER}/_cat/indices/bar?v >>bar.json
#curl -s -XPOST -H "Authorization: Basic $AUTH" http://${SERVER}/bar/_search -H 'Content-Type:application/json' -d'
#{
#	"query": {
#		"match_all": {
#		}
#	}
#}' >>bar.json


#查询30天过期索引
DATE=`date -d "30 days ago" +%Y.%m.%d`
#curl -s -XGET -H "Authorization: Basic $AUTH" http://${SERVER}/_cat/indices?v

# 索引操作
#curl -s -XDELETE -H "Authorization: Basic $AUTH" http://${SERVER}/foo
#curl -s -XPUT -H "Authorization: Basic $AUTH" http://${SERVER}/foo -H 'Content-Type:application/json' -d'
#{
#    "settings" : {
#        "index" : {
#                "number_of_shards": 3,
#                "number_of_replicas": 1   
#        }
#    }
#}'
#curl -s -XGET -H "Authorization: Basic $AUTH" http://${SERVER}/_cat/indices/foo?v
#curl -s -XGET -H "Authorization: Basic $AUTH" http://${SERVER}/_cat/shards/foo?v

#curl -s -XPOST -H "Authorization: Basic $AUTH" http://${SERVER}/foo/fulltext/_mapping -H 'Content-Type:application/json' -d'
#{
#        "properties": {
#            "content": {
#                "type": "text",
#                "analyzer": "ik_max_word",
#                "search_analyzer": "ik_max_word"
#            }
#        }
#}'
#curl -s -XPOST -H "Authorization: Basic $AUTH" http://${SERVER}/foo/fulltext/2 -H 'Content-Type:application/json' -d'
#{"content":"美国留给伊拉克的是个烂摊子吗"}'
#curl -s -XPOST -H "Authorization: Basic $AUTH" http://${SERVER}/foo/fulltext/1 -H 'Content-Type:application/json' -d'
#{"content":"公安部：各地校车将享最高路权"}'
#curl -s -XPOST -H "Authorization: Basic $AUTH" http://${SERVER}/foo/fulltext/3 -H 'Content-Type:application/json' -d'
#{"content":"中韩渔警冲突调查：韩警平均每天扣1艘中国渔船"}'
#curl -s -XPOST -H "Authorization: Basic $AUTH" http://${SERVER}/foo/fulltext/_search -H 'Content-Type:application/json' -d'
#{
#    "query" : { "match" : { "content" : "中国" }},
#    "highlight" : {
#        "pre_tags" : ["<tag1>", "<tag2>"],
#        "post_tags" : ["</tag1>", "</tag2>"],
#        "fields" : {
#            "content" : {}
#        }
#    }
#}'
#curl -s -XGET -H "Authorization: Basic $AUTH" http://${SERVER}/${index}/_search?pretty -H 'Content-Type: application/json' -d'
#{
#  "from" : 0, "size" : 10,
#  "query": { "match_all": {} },
#  "sort": [
#    { "_id": "asc" }
#  ]
#}
#'

##迁移索引
# --httpAuthFile auth.ini
# 拷贝analyzer分词
#elasticdump \
#  --input=http://${USR}@${SERVER}/wyc-carorderimplbootermqconsumer-0723-2021.09.26 \
#  --output=http://${DST}/bar \
#  --type=analyzer
## 拷贝设置
#elasticdump \
#  --input=http://${USR}@${SERVER}/wyc-carorderimplbootermqconsumer-0723-2021.09.26 \
#  --output=http://${DST}/bar \
#  --type=settings
##拷贝映射
#elasticdump \
# --input=http://${USR}@${SERVER}/wyc-carorderimplbootermqconsumer-0723-2021.09.26 \
# --output=http://${DST}/bar \
# --type=mapping
#sleep 1
##拷贝数据
#elasticdump \
# --input=http://${USR}@${SERVER}/wyc-carorderimplbootermqconsumer-0723-2021.09.26 \
# --output=http://${DST}/bar \
# --type=data

#elasticdump \
# --input=http://${USR}@${SERVER}/businessmpl-dev2-kpiserviceimpl-2021.10.07 \
# --output=my_index.json \
# --type=data

 # 拆分索引
#curl -s -XPUT -H "Authorization: Basic $AUTH" http://${SERVER}/ks-dev-log-2021.10.02 -H 'Content-Type:application/json' -d'
#{
#    "settings": {
#        "index.number_of_shards" : 1,
#        "index.number_of_routing_shards": 2 
#    }
#}'
#
#curl -s -XPUT -H "Authorization: Basic $AUTH" http://${SERVER}/ks-dev-log-2021.10.02 -H 'Content-Type:application/json' -d'
#{
#"settings": {
#	"index.blocks.write": true
#}
#}'
##curl -s -XGET -H "Authorization: Basic $AUTH" http://${SERVER}/_cat/recovery?v

# 别名滚动
#ks-dev-log-2021.10.02
#curl -s -XPUT -H "Authorization: Basic $AUTH" http://${SERVER}/ks-dev-log-2021.10.02 -H 'Content-Type:application/json' -d'
#{
#    "aliases": {
#        "logs_write": {}
#    }
#}

#
curl -s -X GET -H "Authorization: Basic $AUTH" http://${SERVER}/_cluster/health?pretty

curl -XGET -H "Authorization: Basic $AUTH" http://${SERVER}/
# 集群健康状态
_cat/health?v
_cluster/health?pretty
# 索引或分片级别
_cluster/health?pretty=true[&level=indices|&level=shards]
# 集群及系统信息
_cluster/stats?pretty=true
# 集群的详细信息。包括节点、分片等
_cluster/state?pretty=true
# 获取集群堆积的任务
_cluster/pending_tasks?pretty=true
# 查询节点情况
_cat/nodes?v
_nodes/stats?pretty=true
_nodes/
# 显示所有indices
_cat/indices
# 磁盘分配
_cat/allocation?v
# 查看索引存储位置
_cat/shards?h=n?
# 分片的索引名和状态
_cat/shards
GET _cat/shards?h=index,shard,prirep,state,unassigned.reason| grep UNASSIGNED
# 索引迁移
curl -X PUT "192.168.1.31:9200/*/_settings?pretty" -H 'Content-Type: application/json' -d'
{
  "index.routing.allocation.include._ip": "192.168.1.51,192.168.1.52,192.168.1.53"
}'
# flush
-X  POST _flush/synced
```

## es配置
```
cluster.info.update.interval // 默认30秒
cluster.routing.allocation.disk.watermark.low // 默认85% 
cluster.routing.allocation.disk.watermark.high // 默认 90%
# 修改分片数据均衡控制策略
curl -XPUT localhost:9200/_cluster/settings -d '{
 
    "transient": {
        "cluster.routing.allocation.disk.watermark.low": "90%",
        "cluster.routing.allocation.disk.watermark.high": "95%"
    }
}'
```
## 文件描述符
```shell
/etc/security/limits.conf
ulimit -n
lsof -p $PID |wc -l
# 限制
cat /proc/$PID/limits | grep "Max open files"
/proc/sys/fs/file-max	//最大所能分配的句柄数(与下面最后一个值同义)
/proc/sys/fs/file-nr	//系统已经分配的文件句柄数	没有用到的句柄数	所有可分配的最大句柄数
systemctl里面也有配置
```

[es集群状态查询](https://blog.csdn.net/genghaihua/article/details/81479619)
[ES shard unassigned的解决方法汇总](https://www.cnblogs.com/bonelee/p/7459509.html)
[unassigned shards](https://www.datadoghq.com/blog/elasticsearch-unassigned-shards/)
[es不更新数据](https://www.cnblogs.com/daemonyue/p/14208159.htmlJ)
[es详细](https://www.cnblogs.com/kevingrace/p/10671063.html)
[es最佳实践](https://www.cnblogs.com/kevingrace/p/10682264.html)
[red cluster](https://www.cnblogs.com/bonelee/p/7459408.html)
[es集群](https://www.cnblogs.com/qcloud1001/p/13755469.html)
[es性能优化](https://www.cnblogs.com/qcloud1001/p/13755469.html)
[es red排查指南](https://www.cnblogs.com/tcy1/p/14293423.html)
[es常用查询](https://www.icode9.com/content-4-772905.html)
[elasticdump](https://github.com/elasticsearch-dump/elasticsearch-dump)
[esdump](https://www.cnblogs.com/sunfie/p/10165473.html)
[es查询不一致](https://cloud.tencent.com/developer/article/1445643)
[es离线索引迁移](https://cloud.tencent.com/developer/article/1145944)
[es template](https://www.elastic.co/guide/en/elasticsearch/reference/6.3/indices-templates.html)
[es 检索深度优化](https://zhuanlan.zhihu.com/p/337981702)
[es排障](https://blog.csdn.net/shen2308/article/details/108548347)
[es负载均衡死锁](https://www.it610.com/article/1282559070670176256.htm)
[es全量读索引](https://cloud.tencent.com/developer/article/1122524)
[tencent es工程师](https://cloud.tencent.com/developer/user/2316162)
[eck](https://github.com/elastic/cloud-on-k8s)
[es 笔记](https://blog.gmem.cc/elasticsearch-study-note)
[number_of_pending_tasks太多](https://zhuanlan.zhihu.com/p/208284617)
[reindex](http://itocm.com/a/EE76894B1304450CA9A65173AB1A0B53)
[shrink](https://www.cnblogs.com/xibuhaohao/p/11158628.html)
[冷热](https://blog.csdn.net/mushao999/article/details/103520079)
[es监控](https://www.cnblogs.com/gered/p/14769403.html)
[es监控类](https://blog.csdn.net/weixin_45413603/article/details/109746006)
[es-py](https://elasticsearch-py.readthedocs.io/en/master/api.html#indices)
