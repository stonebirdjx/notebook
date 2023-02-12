<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [简介（6.0+）](#%E7%AE%80%E4%BB%8B60)
- [nerdctl安装](#nerdctl%E5%AE%89%E8%A3%85)
- [mongo连接](#mongo%E8%BF%9E%E6%8E%A5)
- [DB](#db)
  - [查看数据库](#%E6%9F%A5%E7%9C%8B%E6%95%B0%E6%8D%AE%E5%BA%93)
  - [切换数据库](#%E5%88%87%E6%8D%A2%E6%95%B0%E6%8D%AE%E5%BA%93)
  - [删除数据库](#%E5%88%A0%E9%99%A4%E6%95%B0%E6%8D%AE%E5%BA%93)
  - [只清空文档](#%E5%8F%AA%E6%B8%85%E7%A9%BA%E6%96%87%E6%A1%A3)
- [collections](#collections)
  - [查看集合](#%E6%9F%A5%E7%9C%8B%E9%9B%86%E5%90%88)
  - [创建集合](#%E5%88%9B%E5%BB%BA%E9%9B%86%E5%90%88)
  - [删除集合](#%E5%88%A0%E9%99%A4%E9%9B%86%E5%90%88)
- [CRUD](#crud)
  - [插入文档](#%E6%8F%92%E5%85%A5%E6%96%87%E6%A1%A3)
    - [插入单条数据](#%E6%8F%92%E5%85%A5%E5%8D%95%E6%9D%A1%E6%95%B0%E6%8D%AE)
    - [插入多条数据](#%E6%8F%92%E5%85%A5%E5%A4%9A%E6%9D%A1%E6%95%B0%E6%8D%AE)
    - [其他插入方法](#%E5%85%B6%E4%BB%96%E6%8F%92%E5%85%A5%E6%96%B9%E6%B3%95)
- [操作符](#%E6%93%8D%E4%BD%9C%E7%AC%A6)
- [$type类型](#type%E7%B1%BB%E5%9E%8B)
- [MongoDB 聚合](#mongodb-%E8%81%9A%E5%90%88)
  - [管道聚合](#%E7%AE%A1%E9%81%93%E8%81%9A%E5%90%88)
  - [map-reduce](#map-reduce)
- [模型验证](#%E6%A8%A1%E5%9E%8B%E9%AA%8C%E8%AF%81)
  - [绕过文档验证](#%E7%BB%95%E8%BF%87%E6%96%87%E6%A1%A3%E9%AA%8C%E8%AF%81)
- [MongoDB事务](#mongodb%E4%BA%8B%E5%8A%A1)
  - [多文档事务支持的操作](#%E5%A4%9A%E6%96%87%E6%A1%A3%E4%BA%8B%E5%8A%A1%E6%94%AF%E6%8C%81%E7%9A%84%E6%93%8D%E4%BD%9C)
- [MongoDB索引类型](#mongodb%E7%B4%A2%E5%BC%95%E7%B1%BB%E5%9E%8B)
  - [单字段索引](#%E5%8D%95%E5%AD%97%E6%AE%B5%E7%B4%A2%E5%BC%95)
  - [复合索引](#%E5%A4%8D%E5%90%88%E7%B4%A2%E5%BC%95)
- [MongoDB索引特性](#mongodb%E7%B4%A2%E5%BC%95%E7%89%B9%E6%80%A7)
  - [唯一索引](#%E5%94%AF%E4%B8%80%E7%B4%A2%E5%BC%95)
  - [TTL 索引](#ttl-%E7%B4%A2%E5%BC%95)
  - [部分索引](#%E9%83%A8%E5%88%86%E7%B4%A2%E5%BC%95)
  - [不区分大小写索引](#%E4%B8%8D%E5%8C%BA%E5%88%86%E5%A4%A7%E5%B0%8F%E5%86%99%E7%B4%A2%E5%BC%95)
  - [Sparse索引](#sparse%E7%B4%A2%E5%BC%95)
- [数据库引用](#%E6%95%B0%E6%8D%AE%E5%BA%93%E5%BC%95%E7%94%A8)
  - [安全](#%E5%AE%89%E5%85%A8)
  - [mongo shell中的安全相关方法](#mongo-shell%E4%B8%AD%E7%9A%84%E5%AE%89%E5%85%A8%E7%9B%B8%E5%85%B3%E6%96%B9%E6%B3%95)
    - [用户管理和认证方法](#%E7%94%A8%E6%88%B7%E7%AE%A1%E7%90%86%E5%92%8C%E8%AE%A4%E8%AF%81%E6%96%B9%E6%B3%95)
    - [角色管理方法](#%E8%A7%92%E8%89%B2%E7%AE%A1%E7%90%86%E6%96%B9%E6%B3%95)
    - [安全相关文档](#%E5%AE%89%E5%85%A8%E7%9B%B8%E5%85%B3%E6%96%87%E6%A1%A3)
- [mongodb副本集](#mongodb%E5%89%AF%E6%9C%AC%E9%9B%86)
  - [主节点](#%E4%B8%BB%E8%8A%82%E7%82%B9)
  - [从节点](#%E4%BB%8E%E8%8A%82%E7%82%B9)
  - [副本集日志](#%E5%89%AF%E6%9C%AC%E9%9B%86%E6%97%A5%E5%BF%97)
  - [Replication Methods in the `mongo` Shell](#replication-methods-in-the-mongo-shell)
- [mongodb分片](#mongodb%E5%88%86%E7%89%87)
  - [组件](#%E7%BB%84%E4%BB%B6)
  - [Shell mongo Shell中的分片方法](#shell-mongo-shell%E4%B8%AD%E7%9A%84%E5%88%86%E7%89%87%E6%96%B9%E6%B3%95)
- [MongoDB慢查询日志](#mongodb%E6%85%A2%E6%9F%A5%E8%AF%A2%E6%97%A5%E5%BF%97)
  - [慢查询常用命令](#%E6%85%A2%E6%9F%A5%E8%AF%A2%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)
- [备份和恢复](#%E5%A4%87%E4%BB%BD%E5%92%8C%E6%81%A2%E5%A4%8D)
- [性能](#%E6%80%A7%E8%83%BD)
- [mongodb监控](#mongodb%E7%9B%91%E6%8E%A7)
      - [`mongostat`](#mongostat)
      - [`mongotop`](#mongotop)
  - [MongoDB 包含许多报告数据库状态的命令。](#mongodb-%E5%8C%85%E5%90%AB%E8%AE%B8%E5%A4%9A%E6%8A%A5%E5%91%8A%E6%95%B0%E6%8D%AE%E5%BA%93%E7%8A%B6%E6%80%81%E7%9A%84%E5%91%BD%E4%BB%A4)
      - [`serverStatus`](#serverstatus)
      - [`dbStats`](#dbstats)
      - [`collStats`](#collstats)
      - [`replSetGetStatus`](#replsetgetstatus)
- [存储引擎](#%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E)
  - [WiredTiger 存储引擎](#wiredtiger-%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E)
  - [内存存储引擎](#%E5%86%85%E5%AD%98%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E)
- [参考](#%E5%8F%82%E8%80%83)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 简介（6.0+）

MongoDB中的记录就是一个文档，它是由字段和值对组成的数据结构。 MongoDB 文档类似于 JSON 对象。 字段的值可能包括其他文档、数组和文档数组。

> MongoDB 将数据记录存储为 **BSON 文档**。 BSON 是 JSON 文档的二进制表示，尽管它包含比 JSON 更多的数据类型。

# nerdctl安装

拉取镜像

```bash
nerdctl pull mongo:6.0.3
```

运行镜像

```bash
nerdctl run -d --name stonebird-mongo --restart always -p 27017:27017 -v /root/hjx/mongo/data:/data/db -e MONGO_INITDB_ROOT_USERNAME=root -e MONGO_INITDB_ROOT_PASSWORD=123456 --network stonebird-network --privileged=true mongo:6.0.3
```

# mongo连接

你必须使用 '**mongodb://username:password@hostname/dbname**' 格式

老版本为`mongo`命令,新版本6.0+为`mongosh`

```bash
mongosh mongodb://admin:123456@localhost/
mongosh mongodb://admin:123456@localhost/stonebird
# 连接 replica set 三台服务器
mongosh mongodb://ip1:27017,ip2:27017,ip3:27017
# 以安全模式连接到replica set，并且等待至少两个复制服务器成功写入，超时时间设置为2秒。
mongosh mongodb://host1,host2,host3/?safe=true;w=2;wtimeoutMS=2000
```

**query参数**

| 选项                | 描述                                                         |
| :------------------ | :----------------------------------------------------------- |
| replicaSet=name     | 验证replica set的名称。 Impliesconnect=replicaSet.           |
| slaveOk=true\|false | true:在connect=direct模式下，驱动会连接第一台机器，即使这台服务器不是主。在connect=replicaSet模式下，驱动会发送所有的写请求到主并且把读取操作分布在其他从服务器。false: 在 connect=direct模式下，驱动会自动找寻主服务器. 在connect=replicaSet 模式下，驱动仅仅连接主服务器，并且所有的读写命令都连接到主服务器。 |
| safe=true\|false    | true: 在执行更新操作之后，驱动都会发送getLastError命令来确保更新成功。(还要参考 wtimeoutMS).false: 在每次更新之后，驱动不会发送getLastError来确保更新成功。 |
| w=n                 | 驱动添加 { w : n } 到getLastError命令. 应用于safe=true。     |
| wtimeoutMS=ms       | 驱动添加 { wtimeout : ms } 到 getlasterror 命令. 应用于 safe=true. |
| fsync=true\|false   | true: 驱动添加 { fsync : true } 到 getlasterror 命令.应用于 safe=true.false: 驱动不会添加到getLastError命令中。 |
| journal=true\|false | 如果设置为 true, 同步到 journal (在提交到数据库前写入到实体中). 应用于 safe=true |
| connectTimeoutMS=ms | 可以打开连接的时间。                                         |
| socketTimeoutMS=ms  | 发送和接受sockets的时间。                                    |

# DB

## 查看数据库

```bash
db
```

## 切换数据库

```bash
# 如果数据库不存在，则在您第一次为该数据库存储数据时，MongoDB会创建该数据库。
# use ${db_name}
use stonebird
db.stonebird.insertOne( { name: "stonebird" } )
```

## 删除数据库

```bash
db.dropDatabase()
```

## 只清空文档

```bash
db.stonebird.deleteMany({})
# 老版本可以remove
db.stonebird.remove({})
```

# collections

## 查看集合

可以使用 **show collections** 或 **show tables** 命令

```bash
show collections
show tables
```

## 创建集合

如果集合不存在使用时会自动创建，可以使用`db.createCollection(name,option)`命令创建

```bash
db.createCollection(name,option)
```

option常用字段有 

```bash
{
	size:number, # 固定集合指定一个最大值，即字节数。
	max: number # (可选）指定固定集合中包含文档的最大数量。
}
```

[option字段详情](https://www.mongodb.com/docs/manual/reference/method/db.createCollection/) 

## 删除集合

collection下使用drop命令删除。

```bash
# db.collection.drop()
db.stonebird.drop()
```

# CRUD

## 插入文档

`_id`  字段 存储在集合中的每个文档都需要一个唯一的**_id**字段作为主键。 

如果插入的文档省略**_id**字段，则MongoDB驱动程序会自动为**_id**字段生成**ObjectId**。

### 插入单条数据

使用 `db.collection.insertOne()` 插入单条数据

```bash
db.stonebird.insertOne(
   { item: "canvas", qty: 100, tags: ["cotton"], size: { h: 28, w: 35.5, uom: "cm" } }
)
```

### 插入多条数据

使用`db.collection.insertMany()`可以将多个文档插入一个集合中

```bash
db.stonebird.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], size: { h: 14, w: 21, uom: "cm" } },
   { item: "mat", qty: 85, tags: ["gray"], size: { h: 27.9, w: 35.5, uom: "cm" } },
   { item: "mousepad", qty: 25, tags: ["gel", "blue"], size: { h: 19, w: 22.85, uom: "cm" } }
])
```

### 其他插入方法

这也适用于通过[upsert：true]()通过更新操作插入的文档。

- [`db.collection.updateOne()`](https://www.mongodb.com/docs/upcoming/reference/method/db.collection.updateOne/#mongodb-method-db.collection.updateOne) 
- [`db.collection.updateMany()`](https://www.mongodb.com/docs/upcoming/reference/method/db.collection.updateMany/#mongodb-method-db.collection.updateMany)
- [`db.collection.findAndModify()`](https://www.mongodb.com/docs/upcoming/reference/method/db.collection.findAndModify/#mongodb-method-db.collection.findAndModify) 
- [`db.collection.findOneAndUpdate()`](https://www.mongodb.com/docs/upcoming/reference/method/db.collection.findOneAndUpdate/#mongodb-method-db.collection.findOneAndUpdate) 
- [`db.collection.findOneAndReplace()`](https://www.mongodb.com/docs/upcoming/reference/method/db.collection.findOneAndReplace/#mongodb-method-db.collection.findOneAndReplace) 

或者

[`db.collection.bulkWrite()`](https://www.mongodb.com/docs/upcoming/reference/method/db.collection.bulkWrite/#mongodb-method-db.collection.bulkWrite)

[其他curd详情](https://www.mongodb.com/docs/manual/crud/)

#  操作符

[操作符查询](https://www.mongodb.com/docs/manual/reference/operator/)

# $type类型

| Type                       | Number | Alias                 | Notes                      |
| :------------------------- | :----- | :-------------------- | :------------------------- |
| Double                     | 1      | "double"              |                            |
| String                     | 2      | "string"              |                            |
| Object                     | 3      | "object"              |                            |
| Array                      | 4      | "array"               |                            |
| Binary data                | 5      | "binData"             |                            |
| Undefined                  | 6      | "undefined"           | Deprecated.                |
| ObjectId                   | 7      | "objectId"            |                            |
| Boolean                    | 8      | "bool"                |                            |
| Date                       | 9      | "date"                |                            |
| Null                       | 10     | "null"                |                            |
| Regular Expression         | 11     | "regex"               |                            |
| DBPointer                  | 12     | "dbPointer"           | Deprecated.                |
| JavaScript                 | 13     | "javascript"          |                            |
| Symbol                     | 14     | "symbol"              | Deprecated.                |
| JavaScript code with scope | 15     | "javascriptWithScope" | Deprecated in MongoDB 4.4. |
| 32-bit integer             | 16     | "int"                 |                            |
| Timestamp                  | 17     | "timestamp"           |                            |
| 64-bit integer             | 18     | "long"                |                            |
| Decimal128                 | 19     | "decimal"             |                            |
| Min key                    | -1     | "minKey"              |                            |
| Max key                    | 127    | "maxKey"              |                            |

# MongoDB 聚合

MongoDB 中聚合(aggregate)主要用于处理数据(诸如统计平均值，求和等)，并返回计算后的数据结果。

[官方文档](https://www.mongodb.com/docs/manual/aggregation/)

| SQL Terms, Functions, and Concepts | MongoDB Aggregation Operators                                |
| :--------------------------------- | :----------------------------------------------------------- |
| WHERE                              | [`$match`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/match/#mongodb-pipeline-pipe.-match) |
| GROUP BY                           | [`$group`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/#mongodb-pipeline-pipe.-group) |
| HAVING                             | [`$match`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/match/#mongodb-pipeline-pipe.-match) |
| SELECT                             | [`$project`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project) |
| ORDER BY                           | [`$sort`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#mongodb-pipeline-pipe.-sort) |
| LIMIT                              | [`$limit`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/limit/#mongodb-pipeline-pipe.-limit) |
| SUM()                              | [`$sum`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sum/#mongodb-group-grp.-sum) |
| COUNT()                            | [`$sum`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sum/#mongodb-group-grp.-sum)[`$sortByCount`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sortByCount/#mongodb-pipeline-pipe.-sortByCount) |
| join                               | [`$lookup`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/lookup/#mongodb-pipeline-pipe.-lookup) |
| SELECT INTO NEW_TABLE              | [`$out`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/out/#mongodb-pipeline-pipe.-out) |
| MERGE INTO TABLE                   | [`$merge`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/merge/#mongodb-pipeline-pipe.-merge) (Available starting in MongoDB 4.2) |
| UNION ALL                          | [`$unionWith`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unionWith/#mongodb-pipeline-pipe.-unionWith) (Available starting in MongoDB 4.4) |

## 管道聚合

语法`db.collection.aggregate(pipeline, options)`

```bash
db.orders.aggregate( [
   {
     $group: {
        _id: {
           cust_id: "$cust_id",
           ord_date: {
               month: { $month: "$ord_date" },
               day: { $dayOfMonth: "$ord_date" },
               year: { $year: "$ord_date"}
           }
        }
     }
   },
   {
     $group: {
        _id: null,
        count: { $sum: 1 }
     }
   }
] )
```

sql

```bash
SELECT COUNT(*)
FROM (SELECT cust_id, ord_date
      FROM orders
      GROUP BY cust_id, ord_date)
      as DerivedTable
```



## map-reduce

map-reduce 操作有两个阶段：一个 map 阶段，它处理每个文档并为每个输入文档发出一个或多个对象，以及将map操作的输出组合在一起的_reduce_阶段。可选地，map-reduce 可以具有最终化阶段以对结果进行最终修改。

语法如下

```bash
db.collection.mapReduce(
   function() {emit(key,value);},  //map 函数
   function(key,values) {return reduceFunction},   //reduce 函数
   {
      out: collection,
      query: document,
      sort: document,
      limit: number
   }
)
```

参数说明

- map：映射函数 (生成键值对序列，作为 reduce 函数参数)。
- reduce：统计函数，reduce 函数的任务就是将 key-values 变成 key-value，也就是把 values 数组变成一个单一的值 value。
- out ： 统计结果存放集合 (不指定则使用临时集合,在客户端断开后自动删除)。
- query： 一个筛选条件，只有满足条件的文档才会调用map函数。（query。limit，sort可以随意组合）
- sort： 和 limit 结合的 sort 排序参数（也是在发往map函数前给文档排序），可以优化分组机制
- limit：发往 map 函数的文档数量的上限（要是没有limit，单独使用sort的用处不大）

例子

```javascript
// 创建出 map 方法
var mapFun = function(){
    // 按照 job 分组，取出 price 字段
    emit(this.item, this.price);
};
 
// 创建 reduce 方法
var reduceFun = function(key, values){
    return Array.sum(values)
};
 
// 执行 mapReduce
db.sales.mapReduce(
    mapFun,
    reduceFun,
    {
        out : "sales_1"    //  out 指的是将数据放入 sales_1 这个集合中
    }
);
 
// 返回结果
{
        "result" : "sales_1",
        "timeMillis" : 439,
        "counts" : {
                "input" : 5,
                "emit" : 5,
                "reduce" : 2,
                "output" : 3
        },
        "ok" : 1
}
```

# 模型验证

数据建模的关键挑战是平衡应用程序的需求、数据库引擎的性能特征和数据检索模式

```javascript
db.createCollection("students", {
   validator: {
      $jsonSchema: {
         bsonType: "object",
         required: [ "name", "year", "major", "address" ],
         properties: {
            name: {
               bsonType: "string",
               description: "must be a string and is required"
            },
            year: {
               bsonType: "int",
               minimum: 2017,
               maximum: 3017,
               description: "must be an integer in [ 2017, 3017 ] and is required"
            },
            major: {
               enum: [ "Math", "English", "Computer Science", "History", null ],
               description: "can only be one of the enum values and is required"
            },
            gpa: {
               bsonType: [ "double" ],
               description: "must be a double if the field exists"
            },
            address: {
               bsonType: "object",
               required: [ "city" ],
               properties: {
                  street: {
                     bsonType: "string",
                     description: "must be a string if the field exists"
                  },
                  city: {
                     bsonType: "string",
                     "description": "must be a string and is required"
                  }
               }
            }
         }
      }
   }
})
```

## 绕过文档验证

用户可以使用`bypassDocumentValidation` 选项绕过文档验证。

以下命令可以使用新选项`bypassDocumentValidation`跳过每个操作的验证：

- [`applyOps`](https://docs.mongodb.com/manual/reference/command/applyOps/#dbcmd.applyOps) 命令
- [`findAndModify`](https://docs.mongodb.com/manual/reference/command/findAndModify/#dbcmd.findAndModify) 命令和 [`db.collection.findAndModify()`](https://docs.mongodb.com/manual/reference/method/db.collection.findAndModify/#db.collection.findAndModify) 方法
- [`mapReduce`](https://docs.mongodb.com/manual/reference/command/mapReduce/#dbcmd.mapReduce) 命令和 [`db.collection.mapReduce()`](https://docs.mongodb.com/manual/reference/method/db.collection.mapReduce/#db.collection.mapReduce) 方法
- [`insert`](https://docs.mongodb.com/manual/reference/command/insert/#dbcmd.insert) 命令
- [`update`](https://docs.mongodb.com/manual/reference/command/update/#dbcmd.update) 命令
- 为[`聚合`](https://docs.mongodb.com/manual/reference/command/aggregate/#dbcmd.aggregate) 命令和 [`db.collection.aggregate()`](https://docs.mongodb.com/manual/reference/method/db.collection.aggregate/#db.collection.aggregate) 方法提供的过程命令[`$out`](https://docs.mongodb.com/manual/reference/operator/aggregation/out/#pipe._S_out) 和 [`$merge`](https://docs.mongodb.com/manual/reference/operator/aggregation/merge/#pipe._S_merge)

对于已启用访问控制的部署，若要绕过文档验证，经过身份验证的用户必须具有[`bypassDocumentValidation`](https://docs.mongodb.com/manual/reference/privilege-actions/#bypassDocumentValidation)行动。内置角色[`dbAdmin`](https://docs.mongodb.com/manual/reference/built-in-roles/#dbAdmin) 和 [`restore`](https://docs.mongodb.com/manual/reference/built-in-roles/#restore) 提供此操作。

# MongoDB事务

在MongoDB中，对单个文档的操作是原子的。因为您可以使用嵌入式文档和数组来捕获单个文档结构中的数据之间的关系，而不是在多个文档和集合之间进行规范化，所以这种单文档原子性消除了许多实际用例中对多文档事务的需求。

对于需要原子性地读写多个文档（在单个或多个集合中）的情况，MongoDB支持多文档事务。使用分布式事务，可以跨多个操作，集合，数据库，文档和分片使用事务。

## 多文档事务支持的操作

事务中允许以下读/写操作：

| 方法                                                         | 命令                                                         | 备注                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`db.collection.aggregate()`](https://docs.mongodb.com/manual/reference/method/db.collection.aggregate/#db.collection.aggregate) | [`aggregate`](https://docs.mongodb.com/manual/reference/command/aggregate/#dbcmd.aggregate) | 不包括以下阶段：[`$collStats`](https://docs.mongodb.com/manual/reference/operator/aggregation/collStats/#pipe._S_collStats)[`$currentOp`](https://docs.mongodb.com/manual/reference/operator/aggregation/currentOp/#pipe._S_currentOp)[`$indexStats`](https://docs.mongodb.com/manual/reference/operator/aggregation/indexStats/#pipe._S_indexStats)[`$listLocalSessions`](https://docs.mongodb.com/manual/reference/operator/aggregation/listLocalSessions/#pipe._S_listLocalSessions)[`$listSessions`](https://docs.mongodb.com/manual/reference/operator/aggregation/listSessions/#pipe._S_listSessions)[`$merge`](https://docs.mongodb.com/manual/reference/operator/aggregation/merge/#pipe._S_merge)[`$out`](https://docs.mongodb.com/manual/reference/operator/aggregation/out/#pipe._S_out)[`$planCacheStats`](https://docs.mongodb.com/manual/reference/operator/aggregation/planCacheStats/#pipe._S_planCacheStats) |
| [`db.collection.countDocuments()`](https://docs.mongodb.com/manual/reference/method/db.collection.countDocuments/#db.collection.countDocuments) |                                                              | 不包含以下查询运算符表达式：[`$where`](https://docs.mongodb.com/manual/reference/operator/query/where/#op._S_where)[`$near`](https://docs.mongodb.com/manual/reference/operator/query/near/#op._S_near)[`$nearSphere`](https://docs.mongodb.com/manual/reference/operator/query/nearSphere/#op._S_nearSphere) 。该方法使用[`$match`](https://docs.mongodb.com/manual/reference/operator/aggregation/match/#pipe._S_match)聚合阶段进行查询，并使用[`$group`](https://docs.mongodb.com/manual/reference/operator/aggregation/group/#pipe._S_group)聚合阶段带有[`$sum`](https://docs.mongodb.com/manual/reference/operator/aggregation/sum/#grp._S_sum)表达式来执行计数。 |
| [`db.collection.distinct()`](https://docs.mongodb.com/manual/reference/method/db.collection.distinct/#db.collection.distinct) | [`distinct`](https://docs.mongodb.com/manual/reference/command/distinct/#dbcmd.distinct) | 在未分片集合中可用。对于分片集合，请在 [`$group`](https://docs.mongodb.com/manual/reference/operator/aggregation/group/#pipe._S_group)阶段使用聚合管道。可查看[Distinct Operation](https://docs.mongodb.com/manual/core/transactions-operations/#transactions-operations-distinct)。 |
| [`db.collection.find()`](https://docs.mongodb.com/manual/reference/method/db.collection.find/#db.collection.find) | [`find`](https://docs.mongodb.com/manual/reference/command/find/#dbcmd.find) |                                                              |
|                                                              | [`geoSearch`](https://docs.mongodb.com/manual/reference/command/geoSearch/#dbcmd.geoSearch) |                                                              |
| [`db.collection.deleteMany()`]|<br />(https://docs.mongodb.com/manual/reference/method/db.collection.deleteMany/#db.collection.deleteMany)[`db.collection.deleteOne()`](https://docs.mongodb.com/manual/reference/method/db.collection.deleteOne/#db.collection.deleteOne)[`db.collection.remove()`](https://docs.mongodb.com/manual/reference/method/db.collection.remove/#db.collection.remove) | [`delete`](https://docs.mongodb.com/manual/reference/command/delete/#dbcmd.delete) |                                                              |
| [`db.collection.findOneAndDelete()`](https://docs.mongodb.com/manual/reference/method/db.collection.findOneAndDelete/#db.collection.findOneAndDelete)[`db.collection.findOneAndReplace()`](https://docs.mongodb.com/manual/reference/method/db.collection.findOneAndReplace/#db.collection.findOneAndReplace)[`db.collection.findOneAndUpdate()`](https://docs.mongodb.com/manual/reference/method/db.collection.findOneAndUpdate/#db.collection.findOneAndUpdate) | [`findAndModify`](https://docs.mongodb.com/manual/reference/command/findAndModify/#dbcmd.findAndModify) | 仅在针对现有集合运行时使用`upsert`。                         |
| [`db.collection.insertMany()`](https://docs.mongodb.com/manual/reference/method/db.collection.insertMany/#db.collection.insertMany)[`db.collection.insertOne()`](https://docs.mongodb.com/manual/reference/method/db.collection.insertOne/#db.collection.insertOne)[`db.collection.insert()`](https://docs.mongodb.com/manual/reference/method/db.collection.insert/#db.collection.insert) | [`insert`](https://docs.mongodb.com/manual/reference/command/insert/#dbcmd.insert) | 仅在针对现有集合运行时使用。                                 |
| [`db.collection.save()`](https://docs.mongodb.com/manual/reference/method/db.collection.save/#db.collection.save) |                                                              | 如果插入，则仅在针对现有集合运行时。                         |
| [`db.collection.updateOne()`](https://docs.mongodb.com/manual/reference/method/db.collection.updateOne/#db.collection.updateOne)[`db.collection.updateMany()`](https://docs.mongodb.com/manual/reference/method/db.collection.updateMany/#db.collection.updateMany)[`db.collection.replaceOne()`](https://docs.mongodb.com/manual/reference/method/db.collection.replaceOne/#db.collection.replaceOne)[`db.collection.update()`](https://docs.mongodb.com/manual/reference/method/db.collection.update/#db.collection.update) | [`update`](https://docs.mongodb.com/manual/reference/command/update/#dbcmd.update) | 仅在针对现有集合运行时使用`upsert`。                         |
| [`db.collection.bulkWrite()`](https://docs.mongodb.com/manual/reference/method/db.collection.bulkWrite/#db.collection.bulkWrite)Various [Bulk Operation Methods](https://docs.mongodb.com/manual/reference/method/js-bulk/) |                                                              | 如果插入，则仅在针对现有集合运行时。仅在针对现有集合运行时使用`upsert`。 |

# MongoDB索引类型

索引支持在MongoDB中有效地执行查询。如果没有索引，MongoDB必须执行集合扫描，即扫描集合中的每个文档，以选择那些与查询语句匹配的文档。

```bash
# 查看当前索引
db.user.getIndexes()

# 查看索引大小
db.user.totalIndexSize()

# 删除索引
db.user.dropIndex({"username":1})
db.user.dropIndexes()
```



## 单字段索引

```bash
db.records.createIndex( { score: 1 } )
```

指定一个索引，值**' 1 '**该索引按升序对项目排序。值**' -1 '**指定按降序排列项目的索引。

## 复合索引

```bash
db.user.createIndex({"username":1, "age":-1})
```

该索引被创建后，基于 username 和 age 的查询将会用到该索引，或者是基于 username 的查询也会用到该索引，但是只是基于 age 的查询将不会用到该复合索引。因此可以说， **如果想用到复合索引，必须在查询条件中包含复合索引中的前 N 个索引列**

使用 explain 可以查看是否使用索引

```bash
db.col.find().explain()

# explain executionStats 查询具体的执行时间
db.col.find().explain( "executionStats" )
```

[完整索引文档](https://docs.mongoing.com/indexes)

# MongoDB索引特性

##  唯一索引

```bash
db.user.createIndex({"userid":1},{"unique":true})

# 同样可以创建复合唯一索引，即保证复合键值唯一即可
db.user.createIndex({"userid":1},{"unique":true})
```

## TTL 索引

TTL索引是一种特殊的单字段索引，MongoDB可以使用它在一定的时间或特定的时钟时间后自动从集合中删除文档。

```bash
db.eventlog.createIndex( { "lastModifiedDate": 1 }, { expireAfterSeconds: 3600 } )
```

## 部分索引

部分索引只索引集合中满足指定筛选器表达式的文档。通过索引集合中文档的子集，部分索引可以降低存储需求，并降低创建和维护索引的性能成本。

使用[db.collection.createIndex()](https://docs.mongodb.com/master/reference/db.collection.createindex/ #db.collection.createIndex)方法和**'partialFilterExpression'**选项。**“partialFilterExpression”**选项接受指定筛选条件的文档，使用:

- 等式表达式（即 运算符），`field: value`[`$eq`](https://docs.mongodb.com/master/reference/operator/query/eq/#op._S_eq)
- [`$exists: true`](https://docs.mongodb.com/master/reference/operator/query/exists/#op._S_exists) 表达，
- [`$gt`](https://docs.mongodb.com/master/reference/operator/query/gt/#op._S_gt)，[`$gte`](https://docs.mongodb.com/master/reference/operator/query/gte/#op._S_gte)，[`$lt`](https://docs.mongodb.com/master/reference/operator/query/lt/#op._S_lt)，[`$lte`](https://docs.mongodb.com/master/reference/operator/query/lte/#op._S_lte)表情，
- [`$type`](https://docs.mongodb.com/master/reference/operator/query/type/#op._S_type) 表达式，
- [`$and`](https://docs.mongodb.com/master/reference/operator/query/and/#op._S_and) 只在顶层操作符

例如，下面的操作创建一个复合索引，该索引只对“**rating**”字段大于**5**的文档进行索引。

```bash
db.restaurants.createIndex(
   { cuisine: 1 },
   { partialFilterExpression: { rating: { $gt: 5 } } }
)
```

## 不区分大小写索引

不区分大小写索引支持执行不考虑大小写的字符串比较的查询。

通过指定**“collation”**参数作为选项，可以使用[' db.collection.createIndex() '](https://docs.mongodb.com/master/reference/method/db.collection.createindex/# db.collection.createIndex)创建大小写不敏感索引。例如:

```bash
db.collection.createIndex( { "key" : 1 },
                           { collation: {
                               locale : <locale>,
                               strength : <strength>
                             }
                           } )
```

要为区分大小写的索引指定排序规则，请包括:

- **locale**:指定语言规则。参见[Collation locale](https://docs.mongodb.com/master/reference/coll-locales-defaults/ # coll-languages-locales)获取可用locale列表。
- **strength**:确定比较规则。值“**1**”或“**2**”表示排序规则不区分大小写。

## Sparse索引

Sparse索引只包含有索引字段的文档的条目，即使索引字段包含空值。索引会跳过任何缺少索引字段的文档

```bash
db.addresses.createIndex( { "xmpp_id": 1 }, { sparse: true } )
```

索引不会索引不包含**“xmpp_id”**字段的文档。

#  数据库引用

三个字段表示的意义为：

- $ref：集合名称
- $id：引用的id
- $db:数据库名称，可选参数

```bash
{
   "_id":ObjectId("53402597d852426020000002"),
   "address": {
   "$ref": "address_home",
   "$id": ObjectId("534009e4d852427820000002"),
   "$db": "stonebird"},
   "contact": "987654321",
   "dob": "01-01-1991",
   "name": "Tom Benzamin"
}
```

## 安全

对应用户分配对应权限，启用访问控制

## mongo shell中的安全相关方法

### 用户管理和认证方法

| Name                                                         | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`db.auth()`](https://docs.mongodb.com/manual/reference/method/db.auth/#db.auth) | 向数据库验证用户                                             |
| [`db.changeUserPassword()`](https://docs.mongodb.com/manual/reference/method/db.changeUserPassword/#db.changeUserPassword) | 改变用户的密码                                               |
| [`db.createUser()`](https://docs.mongodb.com/manual/reference/method/db.createUser/#db.createUser) | 创建一个新用户                                               |
| [`db.dropUser()`](https://docs.mongodb.com/manual/reference/method/db.dropUser/#db.dropUser) | 删除一个用户                                                 |
| [`db.dropAllUsers()`](https://docs.mongodb.com/manual/reference/method/db.dropAllUsers/#db.dropAllUsers) | 删除与数据库相关的用户                                       |
| [`db.getUser()`](https://docs.mongodb.com/manual/reference/method/db.getUser/#db.getUser) | 返回指定用户信息                                             |
| [`db.getUsers()`](https://docs.mongodb.com/manual/reference/method/db.getUsers/#db.getUsers) | 返回所有与数据库相关的用户信息                               |
| [`db.grantRolesToUser()`](https://docs.mongodb.com/manual/reference/method/db.grantRolesToUser/#db.grantRolesToUser) | 授予用户角色和角色包含的权限                                 |
| [`db.removeUser()`](https://docs.mongodb.com/manual/reference/method/db.removeUser/#db.removeUser) | 弃用，从数据库删除用户                                       |
| [`db.revokeRolesFromUser()`](https://docs.mongodb.com/manual/reference/method/db.revokeRolesFromUser/#db.revokeRolesFromUser) | 删除用户的角色                                               |
| [`db.updateUser()`](https://docs.mongodb.com/manual/reference/method/db.updateUser/#db.updateUser) | 更新用户数据                                                 |
| [`passwordPrompt()`](https://docs.mongodb.com/manual/reference/method/passwordPrompt/#passwordPrompt) | 提示输入密码，作为在各种mongo shell用户管理方法中直接指定密码的替代方法 |

### 角色管理方法

| Name                                                         | Description                            |
| ------------------------------------------------------------ | -------------------------------------- |
| [`db.createRole()`](https://docs.mongodb.com/manual/reference/method/db.createRole/#db.createRole) | 创建一个角色和指定其权限               |
| [`db.dropRole()`](https://docs.mongodb.com/manual/reference/method/db.dropRole/#db.dropRole) | 删除一个用户自定义角色                 |
| [`db.dropAllRoles()`](https://docs.mongodb.com/manual/reference/method/db.dropAllRoles/#db.dropAllRoles) | 删除与数据库关联的所有用户自定义的角色 |
| [`db.getRole()`](https://docs.mongodb.com/manual/reference/method/db.getRole/#db.getRole) | 返回指定角色的信息                     |
| [`db.getRoles()`](https://docs.mongodb.com/manual/reference/method/db.getRoles/#db.getRoles) | 返回数据库中所有用户自定义角色的信息   |
| [`db.grantPrivilegesToRole()`](https://docs.mongodb.com/manual/reference/method/db.grantPrivilegesToRole/#db.grantPrivilegesToRole) | 给指定用户分配权限                     |
| [`db.revokePrivilegesFromRole()`](https://docs.mongodb.com/manual/reference/method/db.revokePrivilegesFromRole/#db.revokePrivilegesFromRole) | 从用户自定义角色中删除指定权限         |
| [`db.grantRolesToRole()`](https://docs.mongodb.com/manual/reference/method/db.grantRolesToRole/#db.grantRolesToRole) | 指定用户定义的角色从哪些角色继承特权。 |
| [`db.revokeRolesFromRole()`](https://docs.mongodb.com/manual/reference/method/db.revokeRolesFromRole/#db.revokeRolesFromRole) | 从角色中删除继承的角色                 |
| [`db.updateRole()`](https://docs.mongodb.com/manual/reference/method/db.updateRole/#db.updateRole) | 更新用户自定义的角色。                 |

### 安全相关文档

- [system.roles Collection](https://docs.mongodb.com/manual/reference/system-roles-collection/)

  描述存储用户自定义角色的集合的内容。

- [system.users Collection](https://docs.mongodb.com/manual/reference/system-users-collection/)

  描述存储用户凭据和角色分配的集合的内容。

- [Resource Document](https://docs.mongodb.com/manual/reference/resource-document/)

  描述角色的资源文档。

- [Privilege Actions](https://docs.mongodb.com/manual/reference/privilege-actions/)

  可用于权限的操作列表。

# mongodb副本集

一个副本集最多可以有50个成员，但仅能有7个可投票成员。

```bash
mongod --port 27017 --dbpath "D:\set up\mongodb\data" --replSet rs0
#rs.add(HOST_NAME:PORT)
rs.add("mongod1.net:27017")
```

在Mongo客户端使用命令rs.initiate()来启动一个新的副本集。

我们可以使用rs.conf()来查看副本集的配置

查看副本集状态使用 rs.status() 命令

MongoDB中你只能通过主节点将Mongo服务添加到副本集中， 判断当前运行的Mongo服务是否为主节点可以使用命令db.isMaster() 。

## 主节点

副本集的主节点是唯一一个可以接受写操作的成员。MongoDB在[主节点](https://docs.mongodb.com/manual/core/replica-set-members/#replica-set-primary-member) 上应用写操作，然后将这些操作记录到主节点的[oplog](https://docs.mongodb.com/manual/core/replica-set-oplog/)中。[从节点](https://docs.mongodb.com/manual/core/replica-set-members/#replica-set-secondary-members)成员复制这个日志然后应用到它们的数据集中。

副本集所有的成员都可以接受读操作。但是，默认情况下，应用程序会将其读操作定向至主节点。

副本集最多有一个主节点。 如果当前主节点不可用，一个选举会抉择出新的主节点。

## 从节点

一个从节点维护了主节点数据集的一个副本。为了复制数据，从节点通过异步的方式将主节点[oplog](https://docs.mongodb.com/manual/core/replica-set-oplog/) 应用至自己的数据集中。一个副本集可以有一个或多个从节点。

> 一般三副本 为佳

## 副本集日志

**oplog**(操作日志)是一个特殊的集合，它对数据库中所存储数据的所有修改操作进行滚动记录。

```bash
use local
db.oplog.rs.stats().maxSize
```

**日志大小**

当您第一次启动一个副本集成员时，如果您没有指定oplog大小，MongoDB将创建一个默认大小的oplog。[[1\]](https://docs.mongodb.com/manual/core/replica-set-oplog/#oplog)

- 对于Unix和Windows系统

  oplog大小依赖于存储引擎：

  | 存储引擎           | 默认oplog大小    | 下限  | 上限 |
  | ------------------ | ---------------- | ----- | ---- |
  | In-Memory存储引擎  | 物理内存的5%     | 50MB  | 50GB |
  | WiredTiger存储引擎 | 空闲磁盘空间的5% | 990MB | 50GB |

- 对于64-bit macOS系统

  默认的oplog大小是192MB物理内存或空闲磁盘空间，具体取决于存储引擎:

  | 存储引擎           | 默认oplog大小     |
  | ------------------ | ----------------- |
  | In-Memory存储引擎  | 192MB物理内存     |
  | WiredTiger存储引擎 | 192MB空闲磁盘空间 |

## Replication Methods in the `mongo` Shell

[命令参考](https://docs.mongoing.com/replication/replication-reference)

| Name                                                         | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`rs.add()`](https://docs.mongodb.com/v4.2/reference/method/rs.add/#rs.add) | Adds a member to a replica set. 将成员添加到副本集。         |
| [`rs.addArb()`](https://docs.mongodb.com/v4.2/reference/method/rs.addArb/#rs.addArb) | Adds an [arbiter](https://docs.mongodb.com/v4.2/reference/glossary/#term-arbiter) to a replica set. 将仲裁节点添加到副本集。 |
| [`rs.conf()`](https://docs.mongodb.com/v4.2/reference/method/rs.conf/#rs.conf) | Returns the replica set configuration document. 返回副本集的配置内容。 |
| [`rs.freeze()`](https://docs.mongodb.com/v4.2/reference/method/rs.freeze/#rs.freeze) | Prevents the current member from seeking election as primary for a period of time. 阻止当前成员在一段时间内寻求选举为主节点。 |
| [`rs.help()`](https://docs.mongodb.com/v4.2/reference/method/rs.help/#rs.help) | Returns basic help text for [replica set](https://docs.mongodb.com/v4.2/reference/glossary/#term-replica-set) functions. 返回[副本集](https://docs.mongodb.com/v4.2/reference/glossary/#term-replica-set)功能的基本帮助文本。 |
| [`rs.initiate()`](https://docs.mongodb.com/v4.2/reference/method/rs.initiate/#rs.initiate) | Initializes a new replica set. 初始化新的副本集。            |
| [`rs.printReplicationInfo()`](https://docs.mongodb.com/v4.2/reference/method/rs.printReplicationInfo/#rs.printReplicationInfo) | Prints a report of the status of the replica set from the perspective of the primary. 以主节点的角度来打印副本集状态的报告。 |
| [`rs.printSlaveReplicationInfo()`](https://docs.mongodb.com/v4.2/reference/method/rs.printSlaveReplicationInfo/#rs.printSlaveReplicationInfo) | Prints a report of the status of the replica set from the perspective of the secondaries. 以从节点的角度来打印副本集状态的报告。 |
| [`rs.reconfig()`](https://docs.mongodb.com/v4.2/reference/method/rs.reconfig/#rs.reconfig) | Re-configures a replica set by applying a new replica set configuration object. 通过应用新的副本集配置对象来重新配置副本集。 |
| [`rs.remove()`](https://docs.mongodb.com/v4.2/reference/method/rs.remove/#rs.remove) | Remove a member from a replica set. 将成员从副本集中移除。   |
| [`rs.status()`](https://docs.mongodb.com/v4.2/reference/method/rs.status/#rs.status) | Returns a document with information about the state of the replica set. 返回包含关于副本集状态信息的文档。 |
| [`rs.stepDown()`](https://docs.mongodb.com/v4.2/reference/method/rs.stepDown/#rs.stepDown) | Causes the current [primary](https://docs.mongodb.com/v4.2/reference/glossary/#term-primary) to become a secondary which forces an [election](https://docs.mongodb.com/v4.2/reference/glossary/#term-election). 使当前的[主节点](https://docs.mongodb.com/v4.2/reference/glossary/#term-primary)转变为从节点，同时触发[选举](https://docs.mongodb.com/v4.2/reference/glossary/#term-election)。 |
| [`rs.syncFrom()`](https://docs.mongodb.com/v4.2/reference/method/rs.syncFrom/#rs.syncFrom) | Sets the member that this replica set member will sync from, overriding the default sync target selection logic. 设置复制集成员从哪个成员中同步数据，同时覆盖默认的同步目标选择逻辑。 |

# mongodb分片

## 组件

- Shard:

  用于存储实际的数据块，实际生产环境中一个shard server角色可由几台机器组个一个replica set承担，防止主机单点故障

- Config Server:

  mongod实例，存储了整个 ClusterMetadata，其中包括 chunk信息。

- Query Routers（mongos）:

  前端路由，客户端由此接入，且让整个集群看上去像单一数据库，前端应用可以透明使用。

```bash
mongod --port 27020 --dbpath=/www/mongoDB/shard/s0 --logpath=/www/mongoDB/shard/log/s0.log --logappend --fork
mongod --port 27023 --dbpath=/www/mongoDB/shard/s3 --logpath=/www/mongoDB/shard/log/s3.log --logappend --fork

# mongos Routers
mongos --port 40000 --configdb localhost:27100 --fork --logpath=/www/mongoDB/shard/log/route.log --chunkSize 500

# 我们使用MongoDB Shell登录到mongos，添加Shard节点
mongos> db.runCommand({ addshard:"localhost:27020" })
{ "shardAdded" : "shard0000", "ok" : 1 }
mongos> db.runCommand({ addshard:"localhost:27029" })
{ "shardAdded" : "shard0009", "ok" : 1 }
mongos> db.runCommand({ enablesharding:"test" }) #设置分片存储的数据库 必须要跑一下
{ "ok" : 1 }
mongos> db.runCommand({ shardcollection: "test.log", key: { id:1,time:1}})
{ "collectionsharded" : "test.log", "ok" : 1 }
```

> db.runCommand({ enablesharding:"test" }) #设置分片存储的数据库 必须要跑一下

##  Shell mongo Shell中的分片方法

| 名称                     | 描述                                                         |
| ------------------------ | ------------------------------------------------------------ |
| sh.addShard()            | Adds a [shard](https://docs.mongodb.com/v4.2/reference/glossary/#term-shard) to a sharded cluster. 将分片添加到分片集群中。 |
| sh.addShardTag()         | In MongoDB 3.4, this method aliases to [`sh.addShardToZone()`](https://docs.mongodb.com/v4.2/reference/method/sh.addShardToZone/#sh.addShardToZone). 在MongoDB 3.4中，此方法别名为sh.addShardToZone（）。 |
| sh.addShardToZone()      | Associates a shard to a zone. Supports configuring [zones](https://docs.mongodb.com/v4.2/core/zone-sharding/#zone-sharding) in sharded clusters. 将分片与区域关联。支持在分片群集中配置区域。 |
| sh.addTagRange()         | In MongoDB 3.4, this method aliases to [`sh.updateZoneKeyRange()`](https://docs.mongodb.com/v4.2/reference/method/sh.updateZoneKeyRange/#sh.updateZoneKeyRange). 在MongoDB 3.4中，此方法别名为sh.updateZoneKeyRange（）。 |
| sh.disableBalancing()    | Disable balancing on a single collection in a sharded database. Does not affect balancing of other collections in a sharded cluster. 在分片数据库中的单个集合上禁用平衡。不会影响分片集群中其他集合的平衡。 |
| sh.enableBalancing()     | Activates the sharded collection balancer process if previously disabled using [`sh.disableBalancing()`](https://docs.mongodb.com/v4.2/reference/method/sh.disableBalancing/#sh.disableBalancing). 如果以前使用[`sh.disableBalancing()`](https://docs.mongodb.com/v4.2/reference/method/sh.disableBalancing/#sh.disableBalancing)禁用了分片集合平衡器进程，则将其激活。 |
| sh.disableAutoSplit()    | Disables auto-splitting for the sharded cluster. 禁用分片集群的自动拆分。 |
| sh.enableAutoSplit()     | Enables auto-splitting for the sharded cluster. 启用分片集群的自动拆分。 |
| sh.enableSharding()      | Enables sharding on a specific database. 在特定数据库上启用分片。 |
| sh.getBalancerHost()     | *Deprecated since MongoDB 3.4* 自MongoDB 3.4起不推荐使用。   |
| sh.getBalancerState()    | Returns a boolean to report if the [balancer](https://docs.mongodb.com/v4.2/reference/glossary/#term-balancer) is currently enabled. 返回一个布尔值以报告当前是否启用了平衡器。 |
| sh.removeTagRange()      | In MongoDB 3.4, this method aliases to [`sh.removeRangeFromZone()`](https://docs.mongodb.com/v4.2/reference/method/sh.removeRangeFromZone/#sh.removeRangeFromZone). 在MongoDB 3.4中，此方法别名为[`sh.removeRangeFromZone()`](https://docs.mongodb.com/v4.2/reference/method/sh.removeRangeFromZone/#sh.removeRangeFromZone)。 |
| sh.removeRangeFromZone() | Removes an association between a range of shard keys and a zone. Supports configuring [zones](https://docs.mongodb.com/v4.2/core/zone-sharding/#zone-sharding) in sharded clusters. 删除一系列分片键和区域之间的关联。支持在分片集群中配置区域。 |
| sh.help()                | Returns help text for the `sh` methods. 返回sh的帮助文档。   |
| sh.isBalancerRunning()   | Returns a boolean to report if the balancer process is currently migrating chunks. 返回一个布尔值以报告平衡器进程当前是否正在迁移块。 |
| sh.moveChunk()           | Migrates a [chunk](https://docs.mongodb.com/v4.2/reference/glossary/#term-chunk) in a [sharded cluster](https://docs.mongodb.com/v4.2/reference/glossary/#term-sharded-cluster). 迁移分片集群中的块。 |
| sh.removeShardTag()      | In MongoDB 3.4, this method aliases to [`sh.removeShardFromZone()`](https://docs.mongodb.com/v4.2/reference/method/sh.removeShardFromZone/#sh.removeShardFromZone). 在MongoDB 3.4中，此方法别名为sh.removeShardFromZone（）。 |
| sh.removeShardFromZone() | Removes the association between a shard and a zone. Use to manage [zone sharding](https://docs.mongodb.com/v4.2/core/zone-sharding/#zone-sharding). 删除分片和区域之间的关联。用于管理区域分片。 |
| sh.setBalancerState()    | Enables or disables the [balancer](https://docs.mongodb.com/v4.2/reference/glossary/#term-balancer) which migrates [chunks](https://docs.mongodb.com/v4.2/reference/glossary/#term-chunk) between [shards](https://docs.mongodb.com/v4.2/reference/glossary/#term-shard). 启用或禁用在分片之间迁移块的平衡器。 |
| sh.shardCollection()     | Enables sharding for a collection. 为集合启用分片。          |
| sh.splitAt()             | Divides an existing [chunk](https://docs.mongodb.com/v4.2/reference/glossary/#term-chunk) into two chunks using a specific value of the [shard key](https://docs.mongodb.com/v4.2/reference/glossary/#term-shard-key) as the dividing point. 使用分片键的特定值作为分割点将现有的块分为两个块。 |
| sh.splitFind()           | Divides an existing [chunk](https://docs.mongodb.com/v4.2/reference/glossary/#term-chunk) that contains a document matching a query into two approximately equal chunks. 将包含与查询匹配的文档的现有块分为两个大致相等的块。 |
| sh.startBalancer()       | Enables the [balancer](https://docs.mongodb.com/v4.2/reference/glossary/#term-balancer) and waits for balancing to start. 启用平衡器并等待平衡开始。 |
| sh.status()              | Reports on the status of a [sharded cluster](https://docs.mongodb.com/v4.2/reference/glossary/#term-sharded-cluster), as [`db.printShardingStatus()`](https://docs.mongodb.com/v4.2/reference/method/db.printShardingStatus/#db.printShardingStatus). 报告分片群集的状态，如[`db.printShardingStatus()`](https://docs.mongodb.com/v4.2/reference/method/db.printShardingStatus/#db.printShardingStatus)。 |
| sh.stopBalancer()        | Disables the [balancer](https://docs.mongodb.com/v4.2/reference/glossary/#term-balancer) and waits for any in progress balancing rounds to complete. 禁用平衡器，并等待任何进行中的平衡回合完成。 |
| sh.waitForBalancer()     | Internal. Waits for the balancer state to change. 内部。等待平衡器状态更改。 |
| sh.waitForBalancerOff()  | Internal. Waits until the balancer stops running. 内部。等待直到平衡器停止运行。 |
| sh.waitForPingChange()   | Internal. Waits for a change in ping state from one of the [`mongos`](https://docs.mongodb.com/v4.2/reference/program/mongos/#bin.mongos) in the sharded cluster. 内部。等待分片群集中的一个mongos的ping状态更改。 |
| sh.updateZoneKeyRange()  | Associates a range of shard keys to a zone. Supports configuring [zones](https://docs.mongodb.com/v4.2/core/zone-sharding/#zone-sharding) in sharded clusters. 将一系列分片键与区域关联。支持在分片群集中配置区域。 |
| converShardKeyToHashed() | Returns the hashed value for the input. 返回输入的哈希值。   |

# MongoDB慢查询日志

**通过 MongoDB shell 启用**

​    *# 为所有数据库开启慢查询记录*

​    *db.setProfilingLevel(2)*

​    *# 指定数据库，并指定阈值慢查询 ，超过20毫秒的查询被记录*

​    *use testdb.setProfilingLevel(1, { slowms: 20 })*

​    *# 随机采集慢查询的百分比值，sampleRate 值默认为1，表示都采集，0.42 表示采集42%的内容。*

​    *db.setProfilingLevel(1, { sampleRate: 0.42 })*

​    *# 查询慢查询级别和其它信息*

​    *db.getProfilingStatus()*

​    *# 仅返回慢查询级别*

​    *db.getProfilingLevel()*

​    *# 禁用慢查询*

​    *db.setProfilingLevel(0)*

​    **2）通过配置文件启用**

​    在ini 配置文件 mongodb.conf 添加以下参数， profile参数是设置开启等级，slowms是设置阈值

​    *profile = 1*

​    *slowms = 300*

​    **在 YAML配置 文件配置**

​    *operationProfiling:* 

​      *mode:<string># 默认为 off，可选值 off、slowOp(对应上面的等级 1)、all(对应上面的等级 2)*  

​      *slowOpThresholdMs:<int># 阈值，默认值为100，单位毫秒* 

​      *slowOpSampleRate:<double># 随机采集慢查询的百分比值，sampleRate 值默认为1，表示都采集，0.42 表示采集42%的内容*

## 慢查询常用命令

​    *# 查询最近的10个慢查询日志
*

​    db.system.profile.find().limit(10).sort( { ts : -1 } ).pretty()

​    \# 查询除命令类型为 ‘command’ 的日志

​    db.system.profile.find( { op: { $ne : 'command' } } ).pretty()

​    \# 查询数据库为 mydb 集合为 test 的 日志

​    db.system.profile.find( { ns : 'mydb.test' } ).pretty()

​    \# 查询 低于 5毫秒的日志

​    db.system.profile.find( { millis : { $gt : 5 } } ).pretty()

​    \# 查询时间从 2012-12-09 3点整到 2012-12-09 3点40分之间的日志

​    db.system.profile.find({

​      ts : {

​        $gt: new ISODate("2012-12-09T03:00:00Z"),

​        $lt: new ISODate("2012-12-09T03:40:00Z")

​      }

​    }).pretty()

# 备份和恢复

备份

```bash
mongodump -h dbhost -d dbname -o dbdirectory

mongodump --host runoob.com --port 27017
mongodump --dbpath /data/db/ --out /data/backup/
mongodump --collection mycol --db test
```

恢复

```bash
mongorestore -h <hostname><:port> -d dbname <path>
```

# 性能

https://docs.mongoing.com/guan-li/performance

# mongodb监控

#### `mongostat`

[`mongostat`](https://docs.mongodb.com/database-tools/mongostat/#bin.mongostat) 根据数据库操作类型（例如插入，查询，更新，删除等）捕获并返回计数。这些计数报告服务器上的负载分布。

#### `mongotop`

[`mongotop`](https://docs.mongodb.com/database-tools/mongotop/#bin.mongotop)跟踪并报告 MongoDB 实例当前的读写活动，并基于每个集合报告这些统计信息。

## MongoDB 包含许多报告数据库状态的命令。

这些数据可以提供比上面讨论的实用程序更好的粒度级别。您可以考虑在脚本和程序中使用它们的输出来开发自定义警报，或根据实例的活动来修改应用程序的行为。 [`db.currentOp`](https://docs.mongodb.com/manual/reference/method/db.currentOp/#db.currentOp) 方法是用于识别数据库实例正在进行操作的另一有用工具。

#### `serverStatus`

使用 [`serverStatus`](https://docs.mongodb.com/manual/reference/command/serverStatus/#dbcmd.serverStatus) 命令，或shell 程序的[`db.serverStatus()`](https://docs.mongodb.com/manual/reference/method/db.serverStatus/#db.serverStatus) ，可以返回数据库状态的一般概述，包含磁盘使用，内存使用，连接，日志和索引访问。该命令将快速返回，不会影响 MongoDB 的性能。

[`serverStatus`](https://docs.mongodb.com/manual/reference/command/serverStatus/#dbcmd.serverStatus) 输出一个 MongoDB 实例状态的帐户。此命令很少直接运行。在大多数情况下，聚合后的数据更有意义，就像使用监控工具（包括 [MongoDB Cloud Manager](https://www.mongodb.com/cloud/cloud-manager/?tck=docs_server) 和 [Ops Manager](https://www.mongodb.com/products/mongodb-enterprise-advanced?tck=docs_server)）所看到的那样。尽管如此，所有管理员都应该熟悉[`serverStatus`](https://docs.mongodb.com/manual/reference/command/serverStatus/#dbcmd.serverStatus)所提供的数据 。

#### `dbStats`

使用 [`dbStats`](https://docs.mongodb.com/manual/reference/command/dbStats/#dbcmd.dbStats) 命令，或shell 程序的 [`db.stats()`](https://docs.mongodb.com/manual/reference/method/db.stats/#db.stats) ，可以返回一个介绍存储使用和数据量的文档。 [`dbStats`](https://docs.mongodb.com/manual/reference/command/dbStats/#dbcmd.dbStats) 反映存储的使用量，包含在数据库中的数据的数量，对象集合和索引计数器。

使用此数据监视指定数据库的状态和存储容量。此输出还允许您比较数据库之间的使用情况，并确定数据库中[文档](https://docs.mongodb.com/manual/reference/glossary/#term-document)的平均大小。

#### `collStats`

shell 程序的 [`collStats`](https://docs.mongodb.com/manual/reference/command/collStats/#dbcmd.collStats) 或 [`db.collection.stats()`](https://docs.mongodb.com/manual/reference/method/db.collection.stats/#db.collection.stats)提供类似于 [`dbStats`](https://docs.mongodb.com/manual/reference/command/dbStats/#dbcmd.dbStats) 集合级别的统计信息，包括集合中对象的数量，集合的大小，集合使用的磁盘空间量以及有关其索引的信息。

#### `replSetGetStatus`

[`replSetGetStatus`](https://docs.mongodb.com/manual/reference/command/replSetGetStatus/#dbcmd.replSetGetStatus) 命令（来自内核程序的[`rs.status()`](https://docs.mongodb.com/manual/reference/method/rs.status/#rs.status)）可以返回副本集状态的概述。 [replSetGetStatus](https://docs.mongodb.com/manual/reference/command/replSetGetStatus/) 文档详细介绍了副本集和统计信息及其成员的状态和配置。

使用此数据可确保正确配置了复制，并检查了当前主机与副本集的其他成员之间的连接。

# 存储引擎

## WiredTiger 存储引擎

## 内存存储引擎

# 参考

[运算符直达](https://docs.mongoing.com/can-kao)

https://docs.mongoing.com/

https://www.mongodb.com/docs/

