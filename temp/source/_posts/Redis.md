---
title: Redis
date: 2018-03-15 10:51:16
tags: [Redis]
---

study redis from runoob

# Redis 简介

1、简介：REmote DIctionary Server(Redis)是一个key-value存储系统，是一个Key-Value数据库。

2、安装：（Ubuntu下）

```
$sudo apt-get update
$sudo apt-get install redis-server
```

启动：

```
$ redis-server
```

测试：

```
$ redis-cli
redis 127.0.0.1:6379> ping
PONG
```

配置文件：Redis安装目录下，redis.conf

3、五种数据类型：string, hash, list, set, zset(有序集合)

* 举例：string + hash

```
redis> HMSET myhash field1 "Hello" field2 "World"
"OK"
redis> HGET myhash field1
"Hello"
redis> HGET myhash field2
"World"
```

解释：myhash是一个Hash, 有两个键值对，键field1, field2

* 举例：list, 注意左右插入

![list](/images/list.png)

* 举例：set, 返回值 表示成功与否

![](/images/set.png)

* 举例：zset

![zset](/images/zset.png)

* 小结：

  hmset命令操作hash

  `hmset key value`

  push命令：操作列表list

  `lpush/rpush key member`

  sadd命令：操作集合set

  `sadd key member`

  zadd命令 ：操作有序集合zset

  `zadd key score member`

# Redis 命令

### 语法

Redis 客户端的基本语法为：

```
$ redis-cli
```

以下实例讲解了如何启动 redis 客户端：

启动 redis 客户端，打开终端并输入命令 **redis-cli**。该命令会连接本地的 redis 服务。

```
$redis-cli
redis 127.0.0.1:6379>
redis 127.0.0.1:6379> PING

PONG
```

在以上实例中我们连接到本地的 redis 服务并执行 **PING** 命令，该命令用于检测 redis 服务是否启动。

### 在远程服务上执行命令

如果需要在远程 redis 服务上执行命令，同样我们使用的也是 **redis-cli** 命令。

```
$ redis-cli -h host -p port -a password
```

以下实例演示了如何连接到主机为 127.0.0.1，端口为 6379 ，密码为 mypass 的 redis 服务上。

```
$redis-cli -h 127.0.0.1 -p 6379 -a "mypass"
redis 127.0.0.1:6379>
redis 127.0.0.1:6379> PING

PONG
```

*关于更多命令，可以在用的时候边用边学*

[redis命令–runoob](http://www.runoob.com/redis/redis-sets.html)

[redis命令–官方](https://redis.io/commands)

# Redis HyperLogLog

Redis 在 2.8.9 版本添加了 HyperLogLog 结构。

Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基 数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。

但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。

### 什么是基数?

比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。 基数估计就是在误差可接受的范围内，快速计算基数。

### 实例

以下实例演示了 HyperLogLog 的工作过程：

```
redis 127.0.0.1:6379> PFADD runoobkey "redis"

1) (integer) 1

redis 127.0.0.1:6379> PFADD runoobkey "mongodb"

1) (integer) 1

redis 127.0.0.1:6379> PFADD runoobkey "mysql"

1) (integer) 1

redis 127.0.0.1:6379> PFCOUNT runoobkey

(integer) 3
```

后记：学的每个感觉，不学了。。。