<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [简介](#%E7%AE%80%E4%BB%8B)
- [常用](#%E5%B8%B8%E7%94%A8)
  - [基本认证](#%E5%9F%BA%E6%9C%AC%E8%AE%A4%E8%AF%81)
  - [/_cat 公共参数](#_cat-%E5%85%AC%E5%85%B1%E5%8F%82%E6%95%B0)
  - [查看es状态](#%E6%9F%A5%E7%9C%8Bes%E7%8A%B6%E6%80%81)
  - [查看节点列表](#%E6%9F%A5%E7%9C%8B%E8%8A%82%E7%82%B9%E5%88%97%E8%A1%A8)
  - [查看所有索引](#%E6%9F%A5%E7%9C%8B%E6%89%80%E6%9C%89%E7%B4%A2%E5%BC%95)
  - [删除索引](#%E5%88%A0%E9%99%A4%E7%B4%A2%E5%BC%95)
  - [查看各节点磁盘状况](#%E6%9F%A5%E7%9C%8B%E5%90%84%E8%8A%82%E7%82%B9%E7%A3%81%E7%9B%98%E7%8A%B6%E5%86%B5)
  - [查看所有模板](#%E6%9F%A5%E7%9C%8B%E6%89%80%E6%9C%89%E6%A8%A1%E6%9D%BF)
  - [查看某个模板详情](#%E6%9F%A5%E7%9C%8B%E6%9F%90%E4%B8%AA%E6%A8%A1%E6%9D%BF%E8%AF%A6%E6%83%85)
  - [打开/关闭索引](#%E6%89%93%E5%BC%80%E5%85%B3%E9%97%AD%E7%B4%A2%E5%BC%95)
- [:point_right:RESTful API](#point_rightrestful-api)
- [配置](#%E9%85%8D%E7%BD%AE)
  - [动态变更配置](#%E5%8A%A8%E6%80%81%E5%8F%98%E6%9B%B4%E9%85%8D%E7%BD%AE)
  - [日志记录](#%E6%97%A5%E5%BF%97%E8%AE%B0%E5%BD%95)
  - [:point_right:索引性能技巧](#point_right%E7%B4%A2%E5%BC%95%E6%80%A7%E8%83%BD%E6%8A%80%E5%B7%A7)
  - [滚动重启](#%E6%BB%9A%E5%8A%A8%E9%87%8D%E5%90%AF)
  - [:point_right:备份集群](#point_right%E5%A4%87%E4%BB%BD%E9%9B%86%E7%BE%A4)
  - [:point_right:从快照恢复](#point_right%E4%BB%8E%E5%BF%AB%E7%85%A7%E6%81%A2%E5%A4%8D)
  - [elasticsearch.yml](#elasticsearchyml)
- [监控](#%E7%9B%91%E6%8E%A7)
  - [:point_right:集群健康](#point_right%E9%9B%86%E7%BE%A4%E5%81%A5%E5%BA%B7)
  - [:point_right:监控单个节点](#point_right%E7%9B%91%E6%8E%A7%E5%8D%95%E4%B8%AA%E8%8A%82%E7%82%B9)
  - [:point_right:集群统计](#point_right%E9%9B%86%E7%BE%A4%E7%BB%9F%E8%AE%A1)
  - [:point_right:索引统计](#point_right%E7%B4%A2%E5%BC%95%E7%BB%9F%E8%AE%A1)
  - [:point_right:等待中的任务](#point_right%E7%AD%89%E5%BE%85%E4%B8%AD%E7%9A%84%E4%BB%BB%E5%8A%A1)
  - [:point_right:Cat API](#point_rightcat-api)
  - [水平扩容](#%E6%B0%B4%E5%B9%B3%E6%89%A9%E5%AE%B9)
  - [应对故障](#%E5%BA%94%E5%AF%B9%E6%95%85%E9%9A%9C)
- [插入与更新](#%E6%8F%92%E5%85%A5%E4%B8%8E%E6%9B%B4%E6%96%B0)
  - [:point_right:文档元数据](#point_right%E6%96%87%E6%A1%A3%E5%85%83%E6%95%B0%E6%8D%AE)
  - [:point_right:添加索引](#point_right%E6%B7%BB%E5%8A%A0%E7%B4%A2%E5%BC%95)
  - [:point_right:创建新文档](#point_right%E5%88%9B%E5%BB%BA%E6%96%B0%E6%96%87%E6%A1%A3)
  - [:point_right:索引文档](#point_right%E7%B4%A2%E5%BC%95%E6%96%87%E6%A1%A3)
    - [自定义_id](#%E8%87%AA%E5%AE%9A%E4%B9%89_id)
    - [自动生成_id](#%E8%87%AA%E5%8A%A8%E7%94%9F%E6%88%90_id)
  - [:point_right:更新整个文档](#point_right%E6%9B%B4%E6%96%B0%E6%95%B4%E4%B8%AA%E6%96%87%E6%A1%A3)
  - [:point_right:文档的部分更新](#point_right%E6%96%87%E6%A1%A3%E7%9A%84%E9%83%A8%E5%88%86%E6%9B%B4%E6%96%B0)
    - [Groovy 脚本更新](#groovy-%E8%84%9A%E6%9C%AC%E6%9B%B4%E6%96%B0)
  - [:point_right:version - 乐观并发控制](#point_rightversion---%E4%B9%90%E8%A7%82%E5%B9%B6%E5%8F%91%E6%8E%A7%E5%88%B6)
- [查询](#%E6%9F%A5%E8%AF%A2)
  - [:point_right:查询文档数量](#point_right%E6%9F%A5%E8%AF%A2%E6%96%87%E6%A1%A3%E6%95%B0%E9%87%8F)
  - [:point_right:检查文档是否存在](#point_right%E6%A3%80%E6%9F%A5%E6%96%87%E6%A1%A3%E6%98%AF%E5%90%A6%E5%AD%98%E5%9C%A8)
  - [:point_right:查询文档](#point_right%E6%9F%A5%E8%AF%A2%E6%96%87%E6%A1%A3)
    - [:point_right:查询文档的一部分](#point_right%E6%9F%A5%E8%AF%A2%E6%96%87%E6%A1%A3%E7%9A%84%E4%B8%80%E9%83%A8%E5%88%86)
  - [:point_right:Query 轻量搜索](#point_rightquery-%E8%BD%BB%E9%87%8F%E6%90%9C%E7%B4%A2)
  - [:point_right:_mget 查询多个文档](#point_right_mget-%E6%9F%A5%E8%AF%A2%E5%A4%9A%E4%B8%AA%E6%96%87%E6%A1%A3)
  - [:point_right: _search 空搜索](#point_right-_search-%E7%A9%BA%E6%90%9C%E7%B4%A2)
  - [:point_right:分页查询](#point_right%E5%88%86%E9%A1%B5%E6%9F%A5%E8%AF%A2)
  - [:point_right:多索引、多类型](#point_right%E5%A4%9A%E7%B4%A2%E5%BC%95%E5%A4%9A%E7%B1%BB%E5%9E%8B)
  - [:point_right:_bulk 较少代价的批量操作](#point_right_bulk-%E8%BE%83%E5%B0%91%E4%BB%A3%E4%BB%B7%E7%9A%84%E6%89%B9%E9%87%8F%E6%93%8D%E4%BD%9C)
  - [:point_right:查询表达式](#point_right%E6%9F%A5%E8%AF%A2%E8%A1%A8%E8%BE%BE%E5%BC%8F)
    - [match_all](#match_all)
    - [match](#match)
    - [multi_match](#multi_match)
    - [range](#range)
    - [term](#term)
    - [exists、missing](#existsmissing)
  - [组合多查询](#%E7%BB%84%E5%90%88%E5%A4%9A%E6%9F%A5%E8%AF%A2)
    - [**`must`**](#must)
    - [**`must_not`**](#must_not)
    - [**`should`**](#should)
    - [**`filter`**](#filter)
- [映射](#%E6%98%A0%E5%B0%84)
  - [查看映射](#%E6%9F%A5%E7%9C%8B%E6%98%A0%E5%B0%84)
- [删除](#%E5%88%A0%E9%99%A4)
  - [删除文档](#%E5%88%A0%E9%99%A4%E6%96%87%E6%A1%A3)
  - [删除索引](#%E5%88%A0%E9%99%A4%E7%B4%A2%E5%BC%95-1)
- [参考](#%E5%8F%82%E8%80%83)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 简介

Elasticsearch是一个分布式、RESTful的搜索和数据分析引擎的文档数据库。它可以处理海量数据，支持实时搜索和分析，并能够快速地存储和检索数据。Elasticsearch可以在各种场景下使用，例如日志分析、全文搜索、业务指标监控等。它采用Lucene作为其底层引擎，并提供了灵活的查询语言和丰富的聚合功能，使得用户可以轻松地进行各种数据分析和挖掘操作。

ES架构由以下三部分组成：

- 集群（Cluster）
- 节点（Node）：按承担的角色划分为：Master节点、Client节点、Data节点等；
- 分片（Shard）：分为主分片(1)和副本分片(N),每个分片其实都是一个Lucene索引，由于lucene自身的限制，最大包含文档为 Integer.MAX_VALUE-128,2,147,483,519。

另外逻辑意义上：

- 索引的概念，一个集群上的索引，它可能具有多个分片，分布在多个节点上。

> Elasticsearch建立在一个全文搜索引擎库 Apache Lucene™ 基础之上。 Lucene 可以说是当下最先进、高性能、全功能的搜索引擎库—无论是开源还是私有。
>
> Elasticsearch还有一些相关组件，如Kibana用于数据可视化和管理、Logstash用于数据采集和过滤等。这些组件结合在一起可以构建一个完整的数据处理和分析系统。
>
> 因为 Elastic 8 默认开启了 SSL，将默认配置项 xpack.security.http.ssl.enabled: true改为false即可

# 常用

## 基本认证

如果Elasticsearch配置了用户名/密码认证，增加参数**`-u "用户名:密码"`** 即可

```Bash
curl -k -u "elastic:jJJa=KSN2Sy8*MEf+r-k" https://127.0.0.1:9200
```

## /_cat 公共参数

```bash
# v 显示更加详细的信息
GET /_cat/master?v

# help 显示命令结果字段说明
GET /_cat/master?help

# h 显示命令结果想要展示的字段
GET /_cat/master?h=ip,node
GET /_cat/master?h=i*,node

# format 显示命令结果展示格式
# 支持格式类型：text json smile yaml cbor
GET /_cat/indices?format=json&pretty

# s 显示命令结果按照指定字段排序
GET _cat/indices?v&s=index:desc,docs.count:desc
```

## 查看es状态

```bash
curl http://127.0.0.1:9200/_cat/health?v
epoch      timestamp cluster status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shards_percent
1642818699 02:31:39  es-main yellow          2         1     17  17    0    0       17 
```

## 查看节点列表

```bash
curl http://127.0.0.1:9200/_cat/nodes?v
```

## 查看所有索引

```bash
url http://127.0.0.1:9200/_cat/indices?v
```

## 删除索引

```bash
curl -XDELETE http://127.0.0.1:9200/customer
curl -XDELETE http://127.0.0.1:9200/log-time-2022-01.*
```

## 查看各节点磁盘状况

```bash
curl http://127.0.0.1:9200/_cat/allocation?v
```

## 查看所有模板

```bash
curl http://127.0.0.1:9200/_cat/templates
```

## 查看某个模板详情

```bash
curl http://127.0.0.1:9200/_template/k8s-log
```

## 打开/关闭索引

```bash
curl -X POST 'http://127.0.0.1:9200/k8s-log/_open'

curl -X POST 'http://127.0.0.1:9200/k8s-log/_close'
```

关闭的索引不能进行读写操作，几乎不占集群开销。

关闭的索引可以打开，打开走的是正常的恢复流程



# :point_right:RESTful API

RESTful API 通过端口 *9200* 和 Elasticsearch 进行通信

```Bash
curl -X<VERB> [-u uesr:passwd] '<PROTOCOL>://<HOST>:<PORT>/<PATH>?<QUERY_STRING>' -d '<BODY>'
```

# 配置

## 动态变更配置

Elasticsearch 里很多设置都是动态的，可以通过 API 修改。需要强制重启节点（或者集群）的配置修改都要极力避免。而且虽然通过静态配置项也可以完成这些变更，我们建议你还是用 API 来实现。

```bash
PUT /_cluster/settings -d '
{
    "persistent" : {
        "discovery.zen.minimum_master_nodes" : 2 
    },
    "transient" : {
        "indices.store.throttle.max_bytes_per_sec" : "50mb" 
    }
}
'
```

- **临时（Transient）**这些变更在集群重启之前一直会生效。一旦整个集群重启，这些配置就被清除。

- **永久（Persistent）** 这些变更会永久存在直到被显式修改。即使全集群重启它们也会存活下来并覆盖掉静态配置文件里的选项。

## 日志记录

Elasticsearch 会输出很多日志，都放在 `ES_HOME/logs` 目录下。默认的日志记录等级是 `INFO` 。它提供了适度的信息，但是又设计好了不至于让你的日志太过庞大。

你 *可以* 修改 `logging.yml` 文件然后重启你的节点——但是这样做即繁琐还会导致不必要的宕机时间。作为替代，你可以通过 `cluster-settings` API 更新日志记录级别，就像我们前面刚学过的那样。

```bash
PUT /_cluster/settings -d '
{
    "transient" : {
        "logger.discovery" : "DEBUG"
    }
}
'
```

慢日志

```bash
PUT /my_index/_settings -d '
{
    "index.search.slowlog.threshold.query.warn" : "10s", 
    "index.search.slowlog.threshold.fetch.debug": "500ms", 
    "index.indexing.slowlog.threshold.index.info": "5s" 
}
'
```

- 
  查询慢于 10 秒输出一个 WARN 日志。

- 
  获取慢于 500 毫秒输出一个 DEBUG 日志。

- 索引慢于 5 秒输出一个 INFO 日志。

```bash
PUT /_cluster/settings -d '
{
    "transient" : {
        "logger.index.search.slowlog" : "DEBUG", 
        "logger.index.indexing.slowlog" : "WARN" 
    }
}
'
```

- 
  设置搜索慢日志为 DEBUG 级别。

- 
  设置索引慢日志为 WARN 级别

## :point_right:索引性能技巧

合并写入速率,默认值是 20 MB/s，对机械磁盘应该是个不错的设置。如果你用的是 SSD，可以考虑提高到 100–200 MB/s。

```bash
PUT /_cluster/settings -d '
{
    "persistent" : {
        "indices.store.throttle.max_bytes_per_sec" : "100mb"
    }
}
'
```

如果你使用的是机械磁盘而非 SSD，你需要添加下面这个配置到你的 `elasticsearch.yml` 里

```bash
index.merge.scheduler.max_thread_count: 1
```

## 滚动重启

总有一天你会需要做一次集群的滚动重启——保持集群在线和可操作，但是逐一把节点下线。

1. 可能的话，停止索引新的数据。虽然不是每次都能真的做到，但是这一步可以帮助提高恢复速度。

2. 禁止分片分配。这一步阻止 Elasticsearch 再平衡缺失的分片，直到你告诉它可以进行了。如果你知道维护窗口会很短，这个主意棒极了。你可以像下面这样禁止分配：

   ```js
   PUT /_cluster/settings
   {
       "transient" : {
           "cluster.routing.allocation.enable" : "none"
       }
   }
   ```

3. 关闭单个节点。

4. 执行维护/升级。

5. 重启节点，然后确认它加入到集群了。

6. 用如下命令重启分片分配：

   ```js
   PUT /_cluster/settings
   {
       "transient" : {
           "cluster.routing.allocation.enable" : "all"
       }
   }
   ```

   分片再平衡会花一些时间。一直等到集群变成 `绿色` 状态后再继续。

7. 重复第 2 到 6 步操作剩余节点。

8. 到这步你可以安全的恢复索引了（如果你之前停止了的话），不过等待集群完全均衡后再恢复索引，也会有助于提高处理速度。

## :point_right:备份集群

定期备份数据是很重要的

```bash
PUT /_snapshot/my_backup 
{
    "type": "fs", 
    "settings": {
        "location": "/mount/backups/my_backup" 
    }
}
```

一个仓库可以包含多个快照。每个快照跟一系列索引相关

```bash
PUT _snapshot/my_backup/snapshot_1

# 指定备份哪些索引
PUT _snapshot/my_backup/snapshot_2
{
    "indices": "index_1,index_2"
}
```

这个会备份所有打开的索引到 `my_backup` 仓库下一个命名为 `snapshot_1` 的快照里。

列出快照信息

```bash
GET _snapshot/my_backup/snapshot_2
```

删除快照

```bash
DELETE _snapshot/my_backup/snapshot_2
```

## :point_right:从快照恢复

一旦你备份过了数据，恢复它就简单了

```bash
POST _snapshot/my_backup/snapshot_1/_restore
```

## elasticsearch.yml

```yml
cluster.name: my_cluster
node.name: node-1
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch
network.host: 0.0.0.0
http.port: 9200
discovery.seed_hosts: ["192.168.1.100", "192.168.1.101", "192.168.1.102"]
cluster.initial_master_nodes: ["node-1"]
```



# 监控

## :point_right:集群健康

```Bash
GET /_cluster/health

# 响应json
{
   "cluster_name": "elasticsearch_zach",
   "status": "green",
   "timed_out": false,
   "number_of_nodes": 1,
   "number_of_data_nodes": 1,
   "active_primary_shards": 10,
   "active_shards": 10,
   "relocating_shards": 0,
   "initializing_shards": 0,
   "unassigned_shards": 0
}
```

`status` 字段。状态可能是下列三个值之一：

- **`green`**所有的主分片和副本分片都已分配。你的集群是 100% 可用的。
- **`yellow`**所有的主分片已经分片了，但至少还有一个副本是缺失的。不会有数据丢失，所以搜索结果依然是完整的。不过，你的高可用性在某种程度上被弱化。如果 *更多的* 分片消失，你就会丢数据了。把 `yellow` 想象成一个需要及时调查的警告。
- **`red`**至少一个主分片（以及它的全部副本）都在缺失中。这意味着你在缺少数据：搜索只能返回部分数据，而分配到这个分片上的写入请求会返回一个异常。

使用 `level` 参数让 `cluster-health` 显示更详细

```Bash
GET _cluster/health?level=indices

GET _cluster/health?level=shards

# 你可以指定一个 wait_for_status 参数，它只有在状态达标之后才会返回
GET _cluster/health?wait_for_status=green
```

## :point_right:监控单个节点

```Bash
GET /_cat
GET /_nodes/stats
```

## :point_right:集群统计

```Bash
GET /_cluster/stats
```

## :point_right:索引统计

```Bash
GET my_index/_stats 
GET my_index,another_index/_stats 
GET _all/_stats 
```

## :point_right:等待中的任务

```Bash
GET /_cluster/pending_tasks

# 消息体
{
   "tasks": [
      {
         "insert_order": 101,
         "priority": "URGENT",
         "source": "create-index [foo_9], cause [api]",
         "time_in_queue_millis": 86,
         "time_in_queue": "86ms"
      },
      {
         "insert_order": 46,
         "priority": "HIGH",
         "source": "shard-started ([foo_2][1], node[tMTocMvQQgGCkj7QDHl3OA], [P],
         s[INITIALIZING]), reason [after recovery from gateway]",
         "time_in_queue_millis": 842,
         "time_in_queue": "842ms"
      },
      {
         "insert_order": 45,
         "priority": "HIGH",
         "source": "shard-started ([foo_2][0], node[tMTocMvQQgGCkj7QDHl3OA], [P],
         s[INITIALIZING]), reason [after recovery from gateway]",
         "time_in_queue_millis": 858,
         "time_in_queue": "858ms"
      }
  ]
}
```

## :point_right:Cat API

```Bash
GET /_cat

=^.^=
/_cat/allocation
/_cat/shards
/_cat/shards/{index}
/_cat/master
/_cat/nodes
/_cat/indices
/_cat/indices/{index}
/_cat/segments
/_cat/segments/{index}
/_cat/count
/_cat/count/{index}
/_cat/recovery
/_cat/recovery/{index}
/_cat/health
/_cat/pending_tasks
/_cat/aliases
/_cat/aliases/{alias}
/_cat/thread_pool
/_cat/plugins
/_cat/fielddata
/_cat/fielddata/{fields}
```

要启用表头，添加 `?v` 参数即可

```Bash
GET /_cat/health?v

epoch   time    cluster status node.total node.data shards pri relo init
1408[..] 12[..] el[..]  1         1         114 114    0    0     114
unassign
```

`?help` 参数查阅cat文档

```Bash
GET /_cat/nodes?help

id               | id,nodeId               | unique node id
pid              | p                       | process id
host             | h                       | host name
ip               | i                       | ip address
port             | po                      | bound transport port
version          | v                       | es version
build            | b                       | es build hash
jdk              | j                       | jdk version
disk.avail       | d,disk,diskAvail        | available disk space
heap.percent     | hp,heapPercent          | used heap ratio
heap.max         | hm,heapMax              | max configured heap
ram.percent      | rp,ramPercent           | used machine memory ratio
ram.max          | rm,ramMax               | total machine memory
load             | l                       | most recent load avg
uptime           | u                       | node uptime
node.role        | r,role,dc,nodeRole      | d:data node, c:client node
master           | m                       | m:master-eligible, *:current master
```

用 `?h` 参数来明确指定显示这些指标

```Bash
GET /_cat/nodes?v&h=ip,port,heapPercent,heapMax

ip            port heapPercent heapMax
192.168.1.131 9300          53 990.7m

# sort
curl 'localhost:9200/_cat/indices?bytes=b' | sort -rnk8
```

## 水平扩容

如果集群中只有一个节点在运行时，意味着会有一个单点故障问题——没有冗余。 幸运的是，我们只需再启动一个节点即可防止数据丢失。

如果说节点数量超过6个，可以增加副本数提高稳定性。

```bash
PUT /blogs/_settings -d '
{
   "number_of_replicas" : 2
}
'
```

## 应对故障

我们关闭的节点是一个主节点。而集群必须拥有一个主节点来保证正常工作，所以发生的第一件事情就是选举一个新的主节点： `Node 2` 。

# 插入与更新

文档是可以被序列化为包含键值对的 JSON 对象。

## :point_right:文档元数据

三个必须的元数据元素如下：

- **`_index`**：文档在哪存放，一个 *索引* 应该是因共同的特性被分组到一起的文档集合

- **`_type`**：文档表示的对象类别，数据可能在索引中只是松散的组合在一起，但是通常明确定义一些数据中的子分区是很有用的。 例如，所有的产品都放在一个索引中，但是你有许多不同的产品类别，比如 "electronics" 、 "kitchen" 和 "lawn-care"

- **`_id`**：文档唯一标识

## :point_right:添加索引

让我们在包含一个空节点的集群内创建名为 `blogs` 的索引。 索引在默认情况下会被分配5个主分片， 但是为了演示目的，我们将分配3个主分片和一份副本（每个主分片拥有一个副本分片）：

```bash
PUT /blogs -d '
{
   "settings" : {
      "number_of_shards" : 3,
      "number_of_replicas" : 1
   }
}
'
```

> 如果分片没有有效分配，集群的状态为yellow
>
> GET /_cluster/health

## :point_right:创建新文档

```bash
POST /website/blog/ -d '
{ ... }
'

# PUT的方式
PUT /website/blog/123?op_type=create -d'
{ ... }
'

PUT /website/blog/123/_create -d '
{ ... }
'
# 请求成功执行，Elasticsearch 会返回元数据和一个 201 Created 
# 另一方面，如果具有相同的 _index 、 _type 和 _id 的文档已经存在，Elasticsearch 将会返回 409 Conflict 响应码，
{
   "error": {
      "root_cause": [
         {
            "type": "document_already_exists_exception",
            "reason": "[blog][123]: document already exists",
            "shard": "0",
            "index": "website"
         }
      ],
      "type": "document_already_exists_exception",
      "reason": "[blog][123]: document already exists",
      "shard": "0",
      "index": "website"
   },
   "status": 409
}

```

## :point_right:索引文档

一个文档的 `_index` 、 `_type` 和 `_id` 唯一标识一个文档。 我们可以提供自定义的 `_id` 值，或者让 `index` API 自动生成。

```bash
# 自定义文档
PUT /{index}/{type}/{id}
{
  "field": "value",
  ...
}
```

### 自定义_id

```bash
PUT /website/blog/123
{
  "title": "My first blog entry",
  "text":  "Just trying this out...",
  "date":  "2014/01/01"
}

# Elasticsearch 响应体如下所示
{
   "_index":    "website",
   "_type":     "blog",
   "_id":       "123",
   "_version":  1,
   "created":   true
}
```

### 自动生成_id

```bash
# 自动ID POST
POST /website/blog/
{
  "title": "My second blog entry",
  "text":  "Still trying this out...",
  "date":  "2014/01/01"
}

# 响应体如下所示 ,自动生成的 ID 是 URL-safe、 基于 Base64 编码且长度为20个字符的 GUID 字符串。
{
   "_index":    "website",
   "_type":     "blog",
   "_id":       "AVFgSgVHUP18jI2wRx0w",
   "_version":  1,
   "created":   true
}
```

## :point_right:更新整个文档

在 Elasticsearch 中文档是 *不可改变* 的，不能修改它们。需要 *重建索引* 或者进行替换

```bash
PUT /website/blog/123
{
  "title": "My first blog entry",
  "text":  "I am starting to get the hang of this...",
  "date":  "2014/01/02"
}

# 在响应体中，我们能看到 Elasticsearch 已经增加了 _version 字段值：
{
  "_index" :   "website",
  "_type" :    "blog",
  "_id" :      "123",
  "_version" : 2,
  "created":   false  # created制成了false
}
```

发生的动作

- 从旧文档构建 JSON

- 更改该 JSON
- 删除旧文档
- 索引一个新文档

## :point_right:文档的部分更新

`update` 请求最简单的一种形式是接收文档的一部分作为 `doc` 的参数， 它只是与现有的文档进行合并。对象被合并到一起，覆盖现有的字段，增加新的字段。 例如，我们增加字段 `tags` 和 `views` 到我们的博客文章，如下所示：

```bash
POST /website/blog/1/_update
{
   "doc" : {
      "tags" : [ "testing" ],
      "views": 0
   }
}

# 查询
GET /website/blog/1
{
   "_index":    "website",
   "_type":     "blog",
   "_id":       "1",
   "_version":  3,
   "found":     true,
   "_source": {
      "title":  "My first blog entry",
      "text":   "Starting to get the hang of this...",
      "tags": [ "testing" ], 
      "views":  0 
   }
}
```

### Groovy 脚本更新

默认脚本是Groovy，可以通过设置集群中的所有节点的 `config/elasticsearch.yml` 文件来禁用动态 Groovy 脚本：

`script.groovy.sandbox.enabled: false`

**操作_source**

```bash
POST /website/blog/1/_update
{
   "script" : "ctx._source.views+=1"
}
```

**params 使用**

```bash
# 通过使用脚本给 tags 数组添加一个新的标签。
POST /website/blog/1/_update -d '
{
   "script" : "ctx._source.tags+=new_tag",
   "params" : {
      "new_tag" : "search"
   }
}
'
# 更改后如下
{
   "_index":    "website",
   "_type":     "blog",
   "_id":       "1",
   "_version":  5,
   "found":     true,
   "_source": {
      "title":  "My first blog entry",
      "text":   "Starting to get the hang of this...",
      "tags":  ["testing", "search"], 
      "views":  1 
   }
}
```

**可以使用 upsert 参数，指定如果文档不存在就创建它**

```bash
POST /website/pageviews/1/_update -d '
{
   "script" : "ctx._source.views+=1",
   "upsert": {
       "views": 1
   }
}
'
```

## :point_right:version - 乐观并发控制

es 是异步和并发的，这意味着这些复制请求被并行发送，并且到达目的地时也许 *顺序是乱的* 。Elasticsearch 需要一种方法确保文档的旧版本不会覆盖新的版本。

```bash
PUT /website/blog/1/_create -d '
{
  "title": "My first blog entry",
  "text":  "Just trying this out..."
}
'
```

我们指定 version 为我们的修改会被应用的版本

```bash
PUT /website/blog/1?version=1 
{
  "title": "My first blog entry",
  "text":  "Starting to get the hang of this..."
}

# 然而，如果我们重新运行相同的索引请求，仍然指定 version=1 ， Elasticsearch 返回 409 Conflict HTTP 响应码
# 这告诉我们在 Elasticsearch 中这个文档的当前 _version 号是 2 ，但我们指定的更新版本号为 1 。

# 通过增加 version_type=external 到查询字符串的方式重用这些相同的版本号
# 修改版本号为5的值
PUT /website/blog/2?version=5&version_type=external
{
  "title": "My first external blog entry",
  "text":  "Starting to get the hang of this..."
}

# 修改版本号为10的值
PUT /website/blog/2?version=10&version_type=external
{
  "title": "My first external blog entry",
  "text":  "This is a piece of cake..."
}
```

**版本号必须是大于零的整数， 且小于 9.2E+18** 

> 利用 `_version` 号来确保 应用中相互冲突的变更不会导致数据丢失。我们通过指定想要修改文档的 `version` 号来达到这个目的。 如果该版本不是当前版本号，我们的请求将会失败。

# 查询

## :point_right:查询文档数量

```bash
curl 'https://127.0.0.1:9200/_count'

# pretty 格式化输出
curl -X GET 'http://localhost:9200/_count?pretty' -d '
{
    "query": {
        "match_all": {}
    }
}
'
```

## :point_right:检查文档是否存在

如果只想检查一个文档是否存在--根本不想关心内容—那么用 `HEAD` 方法来代替 `GET` 方法。

```bash
curl -i -X HEAD http://localhost:9200/website/blog/123

# 200 存在
# 404 不存在
```

## :point_right:查询文档

为了从 Elasticsearch 中检索出文档，我们仍然使用相同的 `_index` , `_type` , 和 `_id` ，但是 HTTP 谓词更改为 `GET`

```bash
GET /megacorp/employee/1
 
# 返回结果包含了文档的一些元数据，以及 `_source` 属性
{
  "_index" :   "megacorp",
  "_type" :    "employee",
  "_id" :      "1",
  "_version" : 1,
  "found" :    true,
  "_source" :  {
      "first_name" :  "John",
      "last_name" :   "Smith",
      "age" :         25,
      "about" :       "I love to go rock climbing",
      "interests":  [ "sports", "music" ]
  }
}

GET /website/blog/123
GET /website/blog/123?pretty
```

### :point_right:查询文档的一部分

```bash
GET /website/blog/123?_source=title,text

# 只查询_source
GET /website/blog/123?_source
```

> pretty 漂亮的json格式输出

## :point_right:Query 轻量搜索

```bash
#  tweet 字段包含 elasticsearch 单词的所有文档
GET /_all/tweet/_search?q=tweet:elasticsearch
GET /_search?q=date:2014-09-15

# 多个字段 +name:john +tweet:mary 需要url编码
GET /_search?q=%2Bname%3Ajohn+%2Btweet%3Amary

# _all 字段
GET /_search?q=mary
```

## :point_right:_mget 查询多个文档

需要从 Elasticsearch 检索很多文档，那么使用 *multi-get* 或者 `mget` API 来将这些检索请求放在一个请求中，将比逐个文档请求更快地检索到全部文档。

```bash
GET /_mget -d '
{
   "docs" : [
      {
         "_index" : "website",
         "_type" :  "blog",
         "_id" :    2
      },
      {
         "_index" : "website",
         "_type" :  "pageviews",
         "_id" :    1,
         "_source": "views"
      }
   ]
}
'

GET /website/blog/_mget -d '
{
   "docs" : [
      { "_id" : 2 },
      { "_type" : "pageviews", "_id" :   1 }
   ]
}
'
```



## :point_right: _search 空搜索

使用 `_search` 。返回结果包括了所有三个文档，放在数组 `hits` 中 ，hits 一般 返回10条数据

```bash
GET /_search
GET /_search?timeout=10ms
GET /index_2014*/type1,type2/_search
GET /_search -d '
{
  "from": 30,
  "size": 10
}
'


# 返回消息体
{
   "hits" : {
      "total" :       14,
      "hits" : [
        {
          "_index":   "us",
          "_type":    "tweet",
          "_id":      "7",
          "_score":   1,
          "_source": {
             "date":    "2014-09-17",
             "name":    "John Smith",
             "tweet":   "The Query DSL is really powerful and flexible",
             "user_id": 2
          }
       },
        ... 9 RESULTS REMOVED ...
      ],
      "max_score" :   1
   },
   "took" :           4,
   "_shards" : {
      "failed" :      0,
      "successful" :  10,
      "total" :       10
   },
   "timed_out" :      false
}


```

- **hits**: 在 `hits` 数组中每个结果包含文档的 `_index` 、 `_type` 、 `_id` ，加上 `_source` 字段。
- **took** : `took` 值告诉我们执行整个搜索请求耗费了多少毫秒。
- **shards**: `_shards` 部分告诉我们在查询中参与分片的总数，以及这些分片成功了多少个失败了多少个。
- **timeout**: `timed_out` 值告诉我们查询是否超时。默认情况下，搜索请求不会超时。如果低响应时间比完成结果更重要，你可以指定 `timeout` 为 10 或者 10ms（10毫秒），或者 1s（1秒）

## :point_right:分页查询

在 `hits` 数组中只有 10 个文档。如何才能看到其他的文档？类似与SQL的`limits`

- **`size`**显示应该返回的结果数量，默认是 `10`
- **`from`**显示应该跳过的初始结果数量，默认是 `0`

```bash
GET /_search?size=5
GET /_search?size=5&from=5
GET /_search?size=5&from=10

# 例子
curl -X GET "localhost:9200/my_index/_search" -H 'Content-Type: application/json' -d'
{
  "from": 0,
  "size": 10,
  "query": {
    "match_all": {}
  }
}
'
```

## :point_right:多索引、多类型

- /_search 在所有的索引中搜索所有的类型
- /gb/_search 在 gb 索引中搜索所有的类型
- /gb,us/_search  在 gb 和 us 索引中搜索所有的文档
- /g*,u*/_search. 在任何以 g 或者 u 开头的索引中搜索所有的类型
- /gb/user/_search  在 gb 索引中搜索 user 类型
- /gb,us/user,tweet/_search. 在 gb 和 us 索引中搜索 user 和 tweet 类型
- /_all/user,tweet/_search. 在所有的索引中搜索 user 和 tweet 类型

## :point_right:_bulk 较少代价的批量操作

与 `mget` 可以使我们一次取回多个文档同样的方式，

bulk API 允许您将许多操作分组到单个 API 调用中，从而实现更高效的索引操作。这对于大规模索引数据非常有用，可以减少网络延迟并提高索引性能。bulk API 支持三种操作类型：索引、更新和删除。

```bash
curl -XPOST "localhost:9200/my_index/_bulk?pretty" -H "Content-Type: application/json" -d'
{"index":{"_id":"1"}}
{"title":"Document 1","content":"This is the content for document 1."}
{"index":{"_id":"2"}}
{"title":"Document 2","content":"This is the content for document 2."}
{"index":{"_id":"3"}}
{"title":"Document 3","content":"This is the content for document 3."}
'
```

## :point_right:查询表达式

查询表达式(Query DSL)是一种非常灵活又富有表现力的 查询语言。

```bash
GET /_search
{
    "query": YOUR_QUERY_HERE
}
```

### match_all

`match_all` 查询简单的匹配所有文档

```bash
{ "match_all": {}}
```

### match

无论你在任何字段上进行的是全文搜索还是精确查询,`match` 查询是你可用的标准查询

```bash
{ "match": { "tweet": "About Search" }}

# 在一个精确值的字段上使用它，例如数字、日期、布尔或者一个 not_analyzed 字符串字段，那么它将会精确匹配给定的值：
{ "match": { "age":    26           }}
{ "match": { "date":   "2014-09-01" }}
{ "match": { "public": true         }}
{ "match": { "tag":    "full_text"  }
```

### multi_match

`multi_match` 查询可以在多个字段上执行相同的 `match` 查询

```bash
{
    "multi_match": {
        "query":    "full text search",
        "fields":   [ "title", "body" ]
    }
}
```

### range

`range` 查询找出那些落在指定区间内的数字或者时间

```bash
{
    "range": {
        "age": {
            "gte":  20,
            "lt":   30
        }
    }
}
```

- **`gt`**: 大于

- **`gte`**:大于等于

- **`lt`**:小于

- **`lte`**:小于等于

### term

`term` 查询被用于精确值匹配，这些精确值可能是数字、时间、布尔或者那些 `not_analyzed` 的字符串：

```bash
{ "terms": { "tag": [ "search", "full_text", "nosql" ] }}
```

### exists、missing

`exists` 查询和 `missing` 查询被用于查找那些指定字段中有值 (`exists`) 或无值 (`missing`) 的文档。这与SQL中的 `IS_NULL` (`missing`) 和 `NOT IS_NULL` (`exists`) 在本质上具有共性：

```bash
{
    "exists":   {
        "field":    "title"
    }
}
```

## 组合多查询

### **`must`**

文档 *必须* 匹配这些条件才能被包含进来。

### **`must_not`**

文档 *必须不* 匹配这些条件才能被包含进来。

### **`should`**

如果满足这些语句中的任意语句，将增加 `_score` ，否则，无任何影响。它们主要用于修正每个文档的相关性得分。

###  **`filter`**

*必须* 匹配，但它以不评分、过滤模式来进行。这些语句对评分没有贡献，只是根据过滤标准来排除或包含文档。

```bash
{
    "bool": {
        "must":     { "match": { "title": "how to make millions" }},
        "must_not": { "match": { "tag":   "spam" }},
        "should": [
            { "match": { "tag": "starred" }},
            { "range": { "date": { "gte": "2014-01-01" }}}
        ]
    }
}
```

# 映射

为了能够将时间域视为时间，数字域视为数字，字符串域视为全文或精确值字符串， Elasticsearch 需要知道每个域中数据的类型。这个信息包含在映射中。

Elasticsearch 支持如下简单域类型：

- 字符串: `string`
- 整数 : `byte`, `short`, `integer`, `long`
- 浮点数: `float`, `double`
- 布尔型: `boolean`
- 日期: `date`

```Bash
PUT /my_index
{
  "mappings": {
    "properties": {
      "title": { "type": "text" },
      "content": { "type": "text" }
    }
  }
}
```

## 查看映射

```bash
GET /gb/_mapping/tweet
```



# 删除

## 删除文档

```bash
DELETE /website/blog/123

# 查询
GET /website/blog/123
{
  "found" :    false,
  "_index" :   "website",
  "_type" :    "blog",
  "_id" :      "123",
  "_version" : 4
}

# 即使文档不存在（ found 是 false ）， _version 值仍然会增加。这是 Elasticsearch 内部记录本的一部分，用来确保这些改变在跨多节点时以正确的顺序执行。
```

## 删除索引

```bash
curl -XDELETE http://10.8.109.29:9200/customer
```



# 参考

[es中文权威指南](https://www.elastic.co/guide/cn/elasticsearch/guide/current/foreword_id.html)

