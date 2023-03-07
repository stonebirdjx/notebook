<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [redis简介](#redis%E7%AE%80%E4%BB%8B)
  - [redis-server](#redis-server)
    - [:point_right:redis服务相关命令](#point_rightredis%E6%9C%8D%E5%8A%A1%E7%9B%B8%E5%85%B3%E5%91%BD%E4%BB%A4)
  - [redis-cli](#redis-cli)
    - [:point_right:Redis 连接命令](#point_rightredis-%E8%BF%9E%E6%8E%A5%E5%91%BD%E4%BB%A4)
- [切换数据库](#%E5%88%87%E6%8D%A2%E6%95%B0%E6%8D%AE%E5%BA%93)
- [:point_right:配置详情](#point_right%E9%85%8D%E7%BD%AE%E8%AF%A6%E6%83%85)
- [支持数据类型](#%E6%94%AF%E6%8C%81%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)
- [:point_right:keys相关命令](#point_rightkeys%E7%9B%B8%E5%85%B3%E5%91%BD%E4%BB%A4)
- [:point_right:redis服务器命令（特别重要）](#point_rightredis%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%91%BD%E4%BB%A4%E7%89%B9%E5%88%AB%E9%87%8D%E8%A6%81)
- [:point_right:string串操作](#point_rightstring%E4%B8%B2%E6%93%8D%E4%BD%9C)
- [:point_right:hash（map）相关操作](#point_righthashmap%E7%9B%B8%E5%85%B3%E6%93%8D%E4%BD%9C)
- [:point_right:list基本操作](#point_rightlist%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C)
- [:point_right:set 基本操作](#point_rightset-%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C)
- [:point_right:Sorted Sets 有序集合基本操作](#point_rightsorted-sets-%E6%9C%89%E5%BA%8F%E9%9B%86%E5%90%88%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C)
- [基数统计](#%E5%9F%BA%E6%95%B0%E7%BB%9F%E8%AE%A1)
- [地理信息GEO](#%E5%9C%B0%E7%90%86%E4%BF%A1%E6%81%AFgeo)
- [:point_right:Redis Stream-消息队列](#point_rightredis-stream-%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97)
- [:point_right:Redis 发布订阅](#point_rightredis-%E5%8F%91%E5%B8%83%E8%AE%A2%E9%98%85)
- [Redis 事务](#redis-%E4%BA%8B%E5%8A%A1)
- [Redis脚本](#redis%E8%84%9A%E6%9C%AC)
- [:point_right:备份与恢复](#point_right%E5%A4%87%E4%BB%BD%E4%B8%8E%E6%81%A2%E5%A4%8D)
- [:point_right:Redis 持久化](#point_rightredis-%E6%8C%81%E4%B9%85%E5%8C%96)
- [:point_right:redis 安全](#point_rightredis-%E5%AE%89%E5%85%A8)
- [redis 基准测试](#redis-%E5%9F%BA%E5%87%86%E6%B5%8B%E8%AF%95)
- [:point_right:客户端连接](#point_right%E5%AE%A2%E6%88%B7%E7%AB%AF%E8%BF%9E%E6%8E%A5)
- [redis 分区（集群）](#redis-%E5%88%86%E5%8C%BA%E9%9B%86%E7%BE%A4)
  - [主从复制模式](#%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E6%A8%A1%E5%BC%8F)
  - [Sentinel（哨兵）模式](#sentinel%E5%93%A8%E5%85%B5%E6%A8%A1%E5%BC%8F)
  - [Cluster 集群模式 (推荐)](#cluster-%E9%9B%86%E7%BE%A4%E6%A8%A1%E5%BC%8F-%E6%8E%A8%E8%8D%90)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# redis简介

Redis 是完全开源免费的，遵守 BSD 协议，是一个灵活的高性能 key-value 数据结构存储，可以用来作为数据库、缓存和消息队列。

Redis 比其他 key-value 缓存产品有以下三个特点：

- Redis 支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载到内存使用。
- Redis 不仅支持简单的 key-value 类型的数据，同时还提供 list，set，zset，hash 等数据结构的存储。
- Redis 支持主从复制，即 master-slave 模式的数据备份。

Redis 的特点

- **高性能**： Redis 将所有数据集存储在内存中，可以在入门级 Linux 机器中每秒写（SET）11 万次，读（GET）8.1 万次。Redis 支持 Pipelining 命令，可一次发送多条命令来提高吞吐率，减少通信延迟。
- **持久化**：当所有数据都存在于内存中时，可以根据自上次保存以来经过的时间和/或更新次数，使用灵活的策略将更改异步保存在磁盘上。Redis 支持仅附加文件（AOF）持久化模式。
- **数据结构**： Redis 支持各种类型的数据结构，例如字符串、散列、集合、列表、带有范围查询的有序集、位图、超级日志和带有半径查询的地理空间索引。
- **原子操作**：处理不同数据类型的 Redis 操作是原子操作，因此可以安全地 [SET](https://www.redis.com.cn/commands/set.html) 或 [INCR](https://www.redis.com.cn/commands/incr.html) 键，添加和删除集合中的元素等。
- **支持的语言**： Redis 支持许多语言，如 C、C++、Erlang、Go、Haskell、Java、JavaScript（Node.js)、Lua、Objective-C、Perl、PHP、Python、R、Ruby、Rust、Scala、Smalltalk 等。
- **主/从复制**： Redis 遵循非常简单快速的主/从复制。配置文件中只需要一行来设置它，而 Slave 在 Amazon EC2 实例上完成 10 MM key 集的初始同步只需要 21 秒。
- **分片**： Redis 支持分片。与其他键值存储一样，跨多个 Redis 实例分发数据集非常容易。
- **可移植**： Redis 是用 C 编写的，适用于大多数 POSIX 系统，如 Linux、BSD、Mac OS X、Solaris 等。

## redis-server

redis-server 启动一个redis服务

```bash
Usage: ./redis-server [/path/to/redis.conf] [options] [-]
       ./redis-server - (read config from stdin)
       ./redis-server -v or --version
       ./redis-server -h or --help
       ./redis-server --test-memory <megabytes>
       ./redis-server --check-system

Examples:
       ./redis-server (run the server with default conf)
       echo 'maxmemory 128mb' | ./redis-server -
       ./redis-server /etc/redis/6379.conf
       ./redis-server --port 7777
       ./redis-server --port 7777 --replicaof 127.0.0.1 8888
       ./redis-server /etc/myredis.conf --loglevel verbose -
       ./redis-server /etc/myredis.conf --loglevel verbose

Sentinel mode:
       ./redis-server /etc/sentinel.conf --sentinel

# eg
./redis-server ../redis.conf
```

### :point_right:redis服务相关命令

```bash
BGREWRITEAOF	异步执行一个 AOF（AppendOnly File） 文件重写操作
BGSAVE	在后台异步保存当前数据库的数据到磁盘
CLIENT	关闭客户端连接
CLIENT LIST	获取连接到服务器的客户端连接列表
CLIENT GETNAME	获取连接的名称
CLIENT PAUSE	在指定时间内终止运行来自客户端的命令
CLIENT SETNAME	设置当前连接的名称
CLUSTER SLOTS	获取集群节点的映射数组
COMMAND	获取 Redis 命令详情数组
COMMAND COUNT	获取 Redis 命令总数
COMMAND GETKEYS	获取给定命令的所有键
TIME	返回当前服务器时间
COMMAND INFO	获取指定 Redis 命令描述的数组
CONFIG GET	获取指定配置参数的值
CONFIG REWRITE	修改 redis.conf 配置文件
CONFIG SET	修改 redis 配置参数，无需重启
CONFIG RESETSTAT	重置 INFO 命令中的某些统计数据
DBSIZE	返回当前数据库的 key 的数量
DEBUG OBJECT	获取 key 的调试信息
DEBUG SEGFAULT	让 Redis 服务崩溃
FLUSHALL	删除所有数据库的所有 key
FLUSHDB	删除当前数据库的所有 key
INFO	获取 Redis 服务器的各种信息和统计数值
LASTSAVE	返回最近一次 Redis 成功将数据保存到磁盘上的时间
MONITOR	实时打印出 Redis 服务器接收到的命令，调试用
ROLE	返回主从实例所属的角色
SAVE	异步保存数据到硬盘
SHUTDOWN	异步保存数据到硬盘，并关闭服务器
SLAVEOF	将当前服务器转变从属服务器(slave server)
SLOWLOG	管理 redis 的慢日志
SYNC	用于复制功能 ( replication ) 的内部命令
```



## redis-cli

```bash
Usage:
-h <hostname>      Server hostname (default: 127.0.0.1).
-p <port>          Server port (default: 6379).
-s <socket>        Server socket (overrides hostname and port).
-a <password>      Password to use when connecting to the server.
                   You can also use the REDISCLI_AUTH environment
                   variable to pass this password more safely
                   (if both are used, this argument takes precedence).
--user <username>  Used to send ACL style 'AUTH username pass'. Needs -a.
--pass <password>  Alias of -a for consistency with the new --user option.
-r      # 代表将命令重复执行多次
...
redis-cli -h 127.0.0.1 -p 6379 -a ${passwd}
redis-cli -h 127.0.0.1 -p 6379 get hello
redis-cli -r 3 ping
```

### :point_right:Redis 连接命令

```bash
AUTH password	验证密码是否正确
ECHO message	打印字符串
PING	查看服务是否运行
QUIT	关闭当前连接
SELECT index	切换到指定的数据库
```

# 切换数据库

```bash
redis 127.0.0.1:6379> select 1  
redis 127.0.0.1:6379[1]>
```

# :point_right:配置详情

查询 `CONFIG GET`

```bash
CONFIG GET loglevel
CONFIG GET *
```

设置 `CONFIG SET CONFIG_SETTING_NAME NEW_CONFIG_VALUE`

```bash
CONFIG SET CONFIG_SETTING_NAME NEW_CONFIG_VALUE
```

redis.conf 配置项说明

```bash
daemonize no  # Redis 默认不是以守护进程的方式运行
pidfile /var/run/redis.pid # 当 Redis 以守护进程方式运行时，Redis 默认会把 pid 写入 /var/run/redis.pid 文件
port 6379
bind 127.0.0.1
timeout 300 # 当客户端闲置多长时间后关闭连接，如果指定为 0，表示关闭该功能
loglevel verbose # 指定日志记录级别，Redis 总共支持四个级别：debug、verbose、notice、warning，默认为 verbose
logfile stdout # 日志记录方式，默认为标准输出，如果配置 Redis 为守护进程方式运行，而这里又配置为日志记录方式为标准输出，则日志将会发送给 /dev/null
databases 16 # 设置数据库的数量，默认数据库为 0，可以使用 SELECT <dbid> 命令在连接上指定数据库 id
save <seconds> <changes> # 指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合
save 900 1
save 300 10
rdbcompression yes # 指定存储至本地数据库时是否压缩数据，默认为 yes
dbfilename dump.rdb # 指定本地数据库文件名，默认值为 dump.rdb
dir ./ # 指定本地数据库存放目录
slaveof <masterip> <masterport> #设置当本机为 slave 服务时，设置 master 服务的 IP 地址及端口，在 Redis 启动时，它会自动从 master 进行数据同步
masterauth <master-password> # 当 master 服务设置了密码保护时，slave 服务连接 master 的密码
requirepass foobared # 设置 Redis 连接密码，如果配置了连接密码，客户端在连接 Redis 时需要通过 AUTH <password>命令提供密码，默认关闭
maxclients 128 # 设置同一时间最大客户端连接数，默认无限制
其他 # https://www.redis.com.cn/redis-configuration.html
```

# 支持数据类型

Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。

- 位图 ( Bitmaps )
- 基数统计 ( HyperLogLogs )

# :point_right:keys相关命令

```bash
DEL			用于删除 key
DUMP		序列化给定 key ，并返回被序列化的值
EXISTS		检查给定 key 是否存在
EXPIRE		为给定 key 设置过期时间
EXPIREAT	用于为 key 设置过期时间，接受的时间参数是 UNIX 时间戳
PEXPIRE		设置 key 的过期时间，以毫秒计
PEXPIREAT	设置 key 过期时间的时间戳(unix timestamp)，以毫秒计
KEYS		查找所有符合给定模式的 key
MOVE		将当前数据库的 key 移动到给定的数据库中
PERSIST		移除 key 的过期时间，key 将持久保持
PTTL		以毫秒为单位返回 key 的剩余的过期时间
TTL			以秒为单位，返回给定 key 的剩余生存时间(
RANDOMKEY	从当前数据库中随机返回一个 key
RENAME		修改 key 的名称
RENAMENX	仅当 newkey 不存在时，将 key 改名为 newkey
TYPE		返回 key 所储存的值的类型
```

# :point_right:redis服务器命令（特别重要）

```bash
BGREWRITEAOF	异步执行一个 AOF（AppendOnly File） 文件重写操作
BGSAVE	在后台异步保存当前数据库的数据到磁盘
CLIENT	关闭客户端连接
CLIENT LIST	获取连接到服务器的客户端连接列表
CLIENT GETNAME	获取连接的名称
CLIENT PAUSE	在指定时间内终止运行来自客户端的命令
CLIENT SETNAME	设置当前连接的名称
CLUSTER SLOTS	获取集群节点的映射数组
COMMAND	获取 Redis 命令详情数组
COMMAND COUNT	获取 Redis 命令总数
COMMAND GETKEYS	获取给定命令的所有键
TIME	返回当前服务器时间
COMMAND INFO	获取指定 Redis 命令描述的数组
CONFIG GET	获取指定配置参数的值
CONFIG REWRITE	修改 redis.conf 配置文件
CONFIG SET	修改 redis 配置参数，无需重启
CONFIG RESETSTAT	重置 INFO 命令中的某些统计数据
DBSIZE	返回当前数据库的 key 的数量
DEBUG OBJECT	获取 key 的调试信息
DEBUG SEGFAULT	让 Redis 服务崩溃
FLUSHALL	删除所有数据库的所有 key
FLUSHDB	删除当前数据库的所有 key
INFO	获取 Redis 服务器的各种信息和统计数值
LASTSAVE	返回最近一次 Redis 成功将数据保存到磁盘上的时间
MONITOR	实时打印出 Redis 服务器接收到的命令，调试用
ROLE	返回主从实例所属的角色
SAVE	异步保存数据到硬盘
SHUTDOWN	异步保存数据到硬盘，并关闭服务器
SLAVEOF	将当前服务器转变从属服务器(slave server)
SLOWLOG	管理 redis 的慢日志
SYNC	用于复制功能 ( replication ) 的内部命令
```

# :point_right:string串操作

```bash
SET			设置指定 key 的值
GET			获取指定 key 的值
GETRANGE	返回 key 中字符串值的子字符
GETSET		将给定 key 的值设为 value ，并返回 key 的旧值 ( old value )
GETBIT		对 key 所储存的字符串值，获取指定偏移量上的位 ( bit )
MGET		获取所有(一个或多个)给定 key 的值
SETBIT		对 key 所储存的字符串值，设置或清除指定偏移量上的位(bit)
SETEX		设置 key 的值为 value 同时将过期时间设为 seconds
SETNX		只有在 key 不存在时设置 key 的值
SETRANGE	从偏移量 offset 开始用 value 覆写给定 key 所储存的字符串值
STRLEN		返回 key 所储存的字符串值的长度
MSET		同时设置一个或多个 key-value 对
MSETNX		同时设置一个或多个 key-value 对
PSETEX		以毫秒为单位设置 key 的生存时间
INCR		将 key 中储存的数字值增一
INCRBY		将 key 所储存的值加上给定的增量值 ( increment )
INCRBYFLOAT	将 key 所储存的值加上给定的浮点增量值 ( increment )
DECR		将 key 中储存的数字值减一
DECRBY		将 key 所储存的值减去给定的减量值 ( decrement )
APPEND		将 value 追加到 key 原来的值的末尾
```

# :point_right:hash（map）相关操作

Redis hash 是一个 string 类型的 field（字段） 和 value（值） 的映射表，hash 特别适合用于存储对象。

Redis 中每个 hash 可以存储 2**32 - 1 键值对（40多亿）。

```bash
HMSET rediscomcn name "redis" url "http://www.redis.com.cn" rank 1 visitors 240000000
HGETALL rediscomcn
...
# Redis hash 
HDEL	删除一个或多个哈希表字段
HEXISTS	查看哈希表 key 中，指定的字段是否存在
HGET	获取存储在哈希表中指定字段的值
HGETALL	获取在哈希表中指定 key 的所有字段和值
HINCRBY	为哈希表 key 中的指定字段的整数值加上增量 increment
HINCRBYFLOAT	为哈希表 key 中的指定字段的浮点数值加上增量 increment
HKEYS	获取所有哈希表中的字段
HLEN	获取哈希表中字段的数量
HMGET	获取所有给定字段的值
HMSET	同时将多个 field-value (域-值)对设置到哈希表 key 中
HSET	将哈希表 key 中的字段 field 的值设为 value
HSETNX	只有在字段 field 不存在时，设置哈希表字段的值
HVALS	获取哈希表中所有值
HSCAN	迭代哈希表中的键值对
HSTRLEN	返回哈希表 key 中， 与给定域 field 相关联的值的字符串长度
```

#  :point_right:list基本操作

redis 列表是按插入顺序排序的字符串列表。可以在列表的头部（左边）或尾部（右边）添加元素。

列表可以包含超过 40 亿 个元素 ( 2**32 - 1 )。

```bash
LPUSH javatpoint sql  
LPUSH javatpoint mysql 
LRANGE javatpoint 0 10
...

# redis list
BLPOP	移出并获取列表的第一个元素
BRPOP	移出并获取列表的最后一个元素
BRPOPLPUSH	从列表中弹出一个值，并将该值插入到另外一个列表中并返回它
LINDEX	通过索引获取列表中的元素
LINSERT	在列表的元素前或者后插入元素
LLEN	获取列表长度
LPOP	移出并获取列表的第一个元素
LPUSH	将一个或多个值插入到列表头部
LPUSHX	将一个值插入到已存在的列表头部
LRANGE	获取列表指定范围内的元素
LREM	移除列表元素
LSET	通过索引设置列表元素的值
LTRIM	对一个列表进行修剪(trim)
RPOP	移除并获取列表最后一个元素
RPOPLPUSH	移除列表的最后一个元素，并将该元素添加到另一个列表并返回
RPUSH	在列表中添加一个或多个值
RPUSHX	为已存在的列表添加值
```

# :point_right:set 基本操作

Redis 的 Set 是 String 类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据。

```bash
SADD rediscomcn db2  
SADD rediscomcn mongodb 
SMEMBERS rediscomcn
...

# redis set
SADD	向集合添加一个或多个成员
SCARD	获取集合的成员数
SDIFF	返回给定所有集合的差集
SDIFFSTORE	返回给定所有集合的差集并存储在 destination 中
SINTER	返回给定所有集合的交集
SINTERSTORE	返回给定所有集合的交集并存储在 destination 中
SISMEMBER	判断 member 元素是否是集合 key 的成员
SMEMBERS	返回集合中的所有成员
SMOVE	将 member 元素从 source 集合移动到 destination 集合
SPOP	移除并返回集合中的一个随机元素
SRANDMEMBER	返回集合中一个或多个随机数
SREM	移除集合中一个或多个成员
SUNION	返回所有给定集合的并集
SUNIONSTORE	所有给定集合的并集存储在 destination 集合中
SSCAN	迭代集合中的元素
```

# :point_right:Sorted Sets 有序集合基本操作

```bash
ZADD rediscomcn 1 redis 
ZADD rediscomcn 2 cassandra 
ZRANGE rediscomcn 0 10 WITHSCORES  
...

# redis sorted sets
ZADD	向有序集合添加一个或多个成员，或者更新已存在成员的分数
ZCARD	获取有序集合的成员数
ZCOUNT	计算在有序集合中指定区间分数的成员数
ZINCRBY	有序集合中对指定成员的分数加上增量 increment
ZINTERSTORE	计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中
ZLEXCOUNT	在有序集合中计算指定字典区间内成员数量
ZRANGE	通过索引区间返回有序集合成指定区间内的成员
ZRANGEBYLEX	通过字典区间返回有序集合的成员
ZRANGEBYSCORE	通过分数返回有序集合指定区间内的成员
ZRANK	返回有序集合中指定成员的索引
ZREM	移除有序集合中的一个或多个成员
ZREMRANGEBYLEX	移除有序集合中给定的字典区间的所有成员
ZREMRANGEBYRANK	移除有序集合中给定的排名区间的所有成员
ZREMRANGEBYSCORE	移除有序集合中给定的分数区间的所有成员
ZREVRANGE	返回有序集中指定区间内的成员，通过索引，分数从高到底
ZREVRANGEBYSCORE	返回有序集中指定分数区间内的成员，分数从高到低排序
ZREVRANK	返回有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序
ZSCORE	返回有序集中，成员的分数值
ZUNIONSTORE	计算一个或多个有序集的并集，并存储在新的 key 中
ZSCAN	迭代有序集合中的元素（包括元素成员和元素分值）
```

# 基数统计

Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基 数。

比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。

> 统计的是索引

```bash

PFADD rediscomcn "mongodb"
1

# Redis HyperLogLog 
PFADD	添加指定元素到 HyperLogLog 中。
PFCOUNT	返回给定 HyperLogLog 的基数估算值。
PFMERGE	将多个 HyperLogLog 合并为一个 HyperLogLog
```

#  地理信息GEO

```BASH
# GEOADD key longitude latitude member [longitude latitude member ...]
geoadd beijing 116.403856 39.924043 gugong
geoadd beijing 116.343620 39.947633 dongwuyuan
geoadd beijing 116.328643 39.900272 xizhan 116.415324 39.931231 meishuguan 116.416852 39.887607 tiantan
geodist beijing gugong xizhan km # 距离单位可以是 m、km、ml、ft，分别代表米、千米、英里和尺。
geodist beijing gugong dongwuyuan   # 默认单位m

# redis geo
geoadd：添加地理位置的坐标。
geopos：获取地理位置的坐标。
geodist：计算两个位置之间的距离。
georadius：根据用户给定的经纬度坐标来获取指定范围内的地理位置集合。
georadiusbymember：根据储存在位置集合里面的某个地点获取指定范围内的地理位置集合。
geohash：返回一个或多个位置对象的 geohash 值
```

# :point_right:Redis Stream-消息队列

Redis Stream 主要用于消息队列（MQ，Message Queue），Redis 本身是有一个 Redis 发布订阅 (pub/sub) 来实现消息队列的功能，但它有个缺点就是消息无法持久化，如果出现网络断开、Redis 宕机等，消息就会被丢弃。

```bash
# 消息队列相关命令：
XADD - 添加消息到末尾
XTRIM - 对流进行修剪，限制长度
XDEL - 删除消息
XLEN - 获取流包含的元素数量，即消息长度
XRANGE - 获取消息列表，会自动过滤已经删除的消息
XREVRANGE - 反向获取消息列表，ID 从大到小
XREAD - 以阻塞或非阻塞方式获取消息列表
# 消费者组相关命令：
XGROUP CREATE - 创建消费者组
XREADGROUP GROUP - 读取消费者组中的消息
XACK - 将消息标记为"已处理"
XGROUP SETID - 为消费者组设置新的最后递送消息ID
XGROUP DELCONSUMER - 删除消费者
XGROUP DESTROY - 删除消费者组
XPENDING - 显示待处理消息的相关信息
XCLAIM - 转移消息的归属权
XINFO - 查看流和消费者组的相关信息；
XINFO GROUPS - 打印消费者组的信息；
XINFO STREAM - 打印流信息
```

# :point_right:Redis 发布订阅

Redis 发布/订阅是一种消息传模式，其中发送者（在Redis术语中称为发布者）发送消息，而接收者（订阅者）接收消息。传递消息的通道称为**channel**。

```bash
# 第一个客户端
SUBSCRIBE rediscomcnChat
# 第二个客户端
PUBLISH rediscomcnChat "Redis PUBLISH test"

PSUBSCRIBE	订阅一个或多个符合给定模式的频道。
PUBSUB	查看订阅与发布系统状态。
PUBLISH	将信息发送到指定的频道。
PUNSUBSCRIBE	退订所有给定模式的频道。
SUBSCRIBE	订阅给定的一个或多个频道的信息。
UNSUBSCRIBE	指退订给定的频道
```

# Redis 事务

事务是指**一个完整的动作，要么全部执行，要么什么也没有**

Redis 事务不是严格意义上的事务，只是用于帮助用户在一个步骤中执行多个命令。单个 Redis 命令的执行是原子性的，但 Redis 没有在事务上增加任何维持原子性的机制，所以 Redis 事务的执行并不是原子性的。

MULTI、EXEC、DISCARD、WATCH 这四个指令构成了 redis 事务处理的基础。

1. MULTI 用来组装一个事务；
2. EXEC 用来执行一个事务；
3. DISCARD 用来取消一个事务；
4. WATCH 用来监视一些 key，一旦这些 key 在事务执行之前被改变，则取消事务的执行。

```bash
redis 127.0.0.1:6379> MULTI  
OK  
redis 127.0.0.1:6379> SET rediscomcn redis  
QUEUED  
redis 127.0.0.1:6379> GET rediscomcn  
QUEUED  
redis 127.0.0.1:6379> INCR visitors  
QUEUED  
redis 127.0.0.1:6379> EXEC  
1) OK  
2) "redis"  
3) (integer) 1  

# redis 事务
DISCARD	取消事务，放弃执行事务块内的所有命令
EXEC	执行所有事务块内的命令
MULTI	标记一个事务块的开始
UNWATCH	取消 WATCH 命令对所有 key 的监视
WATCH	监视一个(或多个) key
```

# Redis脚本

Redis 脚本使用 Lua 解释器来执行脚本。

后面了解



# :point_right:备份与恢复

使用 SAVE 命令创建当前数据库的备份。

```bash
redis 127.0.0.1:6379> SAVE 
OK
# BGSAVE 后台备份
127.0.0.1:6379> BGSAVE
Background saving started
```

该命令将在 redis 安装目录中创建dump.rdb文件

如果需要恢复数据，只需将备份文件 (dump.rdb) 移动到 redis 安装目录并启动服务即可。

# :point_right:Redis 持久化

redis 提供了两种持久化的方式，分别是**RDB**（Redis DataBase）和**AOF**（Append Only File）。



RDB 方式，是将 redis 某一时刻的数据持久化到磁盘中，是一种快照式的持久化方法。

redis 在进行数据持久化的过程中，会先将数据写入到一个临时文件中，待持久化过程都结束了，才会用这个临时文件替换上次持久化好的文件。正是这种特性，让我们可以随时来进行备份，因为快照文件总是完整可用的。



AOF，英文是 Append Only File，即只允许追加不允许改写的文件。

我们通过配置 redis.conf 中的 appendonly yes 就可以打开 AOF 功能。如果有写操作（如 SET 等），redis 就会被追加到 AOF 文件的末尾。

默认的 AOF 持久化策略是每秒钟 fsync 一次（fsync 是指把缓存中的写指令记录到磁盘中），因为在这种情况下，redis 仍然可以保持很好的处理性能，即使 redis 故障，也只会丢失最近 1 秒钟的数据。



对于我们应该选择 RDB 还是 AOF，官方的建议是两个同时使用。这样可以提供更可靠的持久化方案。

# :point_right:redis 安全

对于数据库来说，安全性是非常必要的，以确保数据的安全性。它提供身份验证，因此如果客户端想要建立连接，则需要在执行命令之前进行身份验证。

您需要在配置文件中设置密码以保护 Redis 数据库。

```bash
127.0.0.1:6379> AUTH password

# CONFIG set requirepass 设置密码
127.0.0.1:6379> CONFIG set requirepass "runoob"
OK
127.0.0.1:6379> CONFIG get requirepass
1) "requirepass"
2) "runoob"
```

# redis 基准测试

```bash
redis-benchmark -h 127.0.0.1 -p 6379 -t set,lpush -n 100000 -q

-h	指定服务器主机名	127.0.0.1
-p	指定服务器端口	6379
-s	指定服务器 socket	
-c	指定并发连接数	50
-n	指定请求数	10000
-d	以字节的形式指定 SET/GET 值的数据大小	2
-k	1=keep alive 0=reconnect	1
-r	SET/GET/INCR 使用随机 key, SADD 使用随机值	
-P	通过管道传输 <numreq> 请求	1
-q	强制退出 redis。仅显示 query/sec 值	
--csv	以 CSV 格式输出	
-l	生成循环，永久执行测试	
-t	仅运行以逗号分隔的测试命令列表	
-I	Idle 模式。仅打开 N 个 idle 连接并等待	
```

# :point_right:客户端连接

```bash
CLIENT LIST	返回连接到 redis 服务的客户端列表
CLIENT SETNAME	设置当前连接的名称
CLIENT GETNAME	获取通过 CLIENT SETNAME 命令设置的服务名称
CLIENT PAUSE	挂起客户端连接，指定挂起的时间以毫秒计
CLIENT KILL	关闭客户端连接
```

# redis 分区（集群）

redis 中有两种类型的分区：

- 范围分区
- 哈希分区

范围分区是执行分区的最简单方法之一。它通过将对象的范围映射到特定的 Redis 实例来完成。

1、配置文件

```ba
port 6379
daemonize yes   # 以后台模式运行
protected-mode no   # 集群模式下要设置成no，设置为yes的时候需要配合bind参数，只有被bind的ip能访问我们的redis

dir /data/redis # 日记、aof、rdb文件都会保存到这里 所以这个路径要是机器磁盘空间最大的（可通过df –h来获取磁盘信息）
logfile redis-6379.log  # 产生的日记名
loglevel notice #日记级别（用于生产环境）

timeout 1800    # 在客户端空闲多久后关闭连接，单位为秒。0表示永不关闭，这里的值必须大于客户端设置的连接池的最小空闲时间
tcp-keepalive 0 # 0表示在没有通信的情况下，不会向客户端发送TCP ACK来检测客户端是否被关闭，因为在客户端有空闲检测，所以服务端没必要去检测客户端的状态

maxmemory 4gb   # redis最多能用多少内存，如果不设置的话，redis会一直消耗完系统所有的内存
maxmemory-policy volatile-lfu    # redis达到maxmemory后的内存回收策略，lfu比lru性能更好
lfu-log-factor 10
lfu-decay-time 1

dbfilename redis-6379.rdb # 产生的rdb文件名
rdbcompression yes  # 开启rdb文件压缩
stop-writes-on-bgsave-error yes  # bgsave错误的时候停止写操作来保证bgsave成功
rdbchecksum yes # 检测rdb文件的完整性

appendonly yes  # 开启aof，建议主节点关闭，从节点开启
appendfsync everysec    # aof刷盘策略
auto-aof-rewrite-min-size 64mb  # 当aof文件多大的时候才进行重写
auto-aof-rewrite-percentage 100 # aof增长率
no-appendfsync-on-rewrite yes   # 在aof重写的时候时候不进行正常的aof
appendfilename redis-6379.aof    # 产生的aof文件名

# 当hash的大小小于512个，并且每个值都小于64byte时，就使用ziplist存储
hash-max-ziplist-entries 512
hash-max-ziplist-value 64
list-max-ziplist-size -2    # redis的list结构是quickList，每个quickList由多个quicklistNode组成，每个quicklistNode有个指针指向实际存储的zipList。这里的-2是指zipList的长度8kb，超出了这个字节数，就会新起一个ziplist
list-compress-depth 0   # 压缩深度为0，表示zipList不压缩，不压缩的话，push/pop会快；深度为1时，表示quicklist首尾两个ziplist不压缩，其他的都压缩；如果深度为2，就表示quicklist的首尾第一个ziplist 以及首尾第二个ziplist都不压缩，其他的都压缩；以此类推
set-max-intset-entries 512  # 当intset的元素个数达到512个后，intset升级成dict
zset-max-ziplist-entries 128    # 与hash同理，因为set是hash的特殊情况，set的value都是null
zset-max-ziplist-value 64
# hll-sparse-max-bytes 3000

slowlog-max-len 1000    # 慢查询队列的长度
slowlog-log-slower-than 1000   # 多少时间定义为慢查询 单位微秒

cluster-enabled yes # 是否以集群的形式启动
cluster-config-file redis-nodes.conf
cluster-require-full-coverage no    # 集群中是否16384个槽都可用或所有master节点都没有问题才对外提供服务，保证集群的完整性 
cluster-node-timeout 15000    # 各个节点相互发送消息的频率，单位为毫秒。某节点发现与其他节点最后通信时间超过cluster-node-timeout/2时会发送ping命令，同时带上slots槽数组（2KB）和整个集群1/10的状态数据（10个节点的状态数据约1KB），该参数也会影响故障转移时间

client-output-buffer-limit normal 0 0 0 # 不限制普通客户端缓冲区
client-output-buffer-limit replica 512mb 128mb 60   # slave同步主节点的数据，当slave缓冲区超过512m或者缓冲区在60s秒一直处于128m以上，slave节点会被挂掉
client-output-buffer-limit pubsub 32mb 8mb 60

replica-lazy-flush yes  # 从库接收完rdb文件后的 flush操作
lazyfree-lazy-eviction yes  # 内存达到 maxmemory时进行淘汰
lazyfree-lazy-expire yes    # key过期删除
lazyfree-lazy-server-del yes    # rename指令删除destKey
```

2、启动

```bash
redis-server redis.conf
```

节点创建完毕后，各个节点实际上是独立的，并没有组成一个集群，还需要下面的操作。

3、创建集群

```bash
redis-cli --cluster create 192.168.1.1:6379 192.168.1.2:6379 192.168.1.3:6379 192.168.1.4:6379 192.168.1.5:6379 192.168.1.6:6379 --cluster-replicas 1
```

**Redis 支持三种集群方案**

- 主从复制模式
- Sentinel（哨兵）模式
- Cluster 模式

## 主从复制模式

通过持久化功能，Redis保证了即使在服务器重启的情况下也不会丢失（或少量丢失）数据，因为持久化会把内存中数据保存到硬盘上，重启会从硬盘上加载数据。 但是由于数据是存储在一台服务器上的，如果这台服务器出现硬盘故障等问题，也会导致数据丢失。

为此， **Redis 提供了复制（replication）功能，可以实现当一台数据库中的数据更新后，自动将更新的数据同步到其他数据库上**。

## Sentinel（哨兵）模式

哨兵模式是一种特殊的模式，首先 Redis 提供了哨兵的命令，**哨兵是一个独立的进程，作为进程，它会独立运行。其原理是哨兵通过发送命令，等待Redis服务器响应，从而监控运行的多个 Redis 实例**。

## Cluster 集群模式 (推荐)

Redis Cluster是一种服务器 Sharding 技术，3.0版本开始正式提供。Redis 的哨兵模式基本已经可以实现高可用，读写分离 ，

**集群的数据分片**

Redis 集群没有使用一致性 hash，而是引入了哈希槽【hash slot】的概念。

Redis 集群有16384 个哈希槽，每个 key 通过 CRC16 校验后对 16384 取模来决定放置哪个槽。集群的每个节点负责一部分hash槽，举个例子，比如当前集群有3个节点，那么：

- 节点 A 包含 0 到 5460 号哈希槽
- 节点 B 包含 5461 到 10922 号哈希槽
- 节点 C 包含 10923 到 16383 号哈希槽
