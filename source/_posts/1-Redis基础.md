---
title: Redis基础
date: 2022-01-31
updated: 2022-02-25
categories:
- Redis
tags:
- Redis
---

<escape><!--more--></escape>

### 安装使用

安装：[Redis 安装 | 菜鸟教程 (runoob.com)](https://www.runoob.com/redis/redis-install.html)

推荐视频：[【尚硅谷】Redis 6 入门到精通 超详细 教程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Rv41177Af?from=search&seid=8211878355588508937&spm_id_from=333.337.0.0)

使用：

1. 打开一个 **cmd** 窗口 使用 cd 命令切换目录到 **C:\redis** 运行

   `redis-server.exe redis.windows.conf`

2. 另启一个 **cmd** 窗口，原来的不要关闭，不然就无法访问服务端

   `redis-cli.exe -h 127.0.0.1 -p 6379`

### 基础操作

1. 默认 `16` 个数据库，初始默认使用 `0` 号库

2. 统一密码管理，所有库的密码相同

3. `key` 键操作

   ````sql
   keys * 		查看当前库所有 key 
   exists key	判断是否存在某 key
   type key	查看某 key 的类型
   del key		删除指定的 key
   unlink key	根据 value 选择非阻塞删除
   expire key	为给定 key 设置过期时间
   ttl key		查看还有多少秒 key 过期	-1表示永不,-2表示已过期
   select key	命令切换数据库
   dbsize		查看当前数据库的 key 数量
   flushdb		清空当前数据库
   flushall	清空所有数据库
   ````

### 数据类型

#### <strong>String 字符串类型</strong>

String 的数据结构为简单动态字符串 **（Simple Dynamic String - SDS）**，是可修改的字符串，

尽管 Redis 是用 C 写的，但是并未使用 C 的字符串表示，而是对char*进行了简单的包装，

采用预分配冗余空间的方式来减少内存的频繁分配，

扩容是加倍现有的空间，但是最大长度为 **512M**

* <a href="https://blog.csdn.net/qq_43561507/article/details/109148830">C语言字符串和 SDS 之间的区别</a>
* <a href="https://blog.csdn.net/darker0019527/article/details/102995888">二者之间的详细对比</a>

   ```sql
   set key value 			添加键值对，重复则会覆盖
   get key				查询对应键值
   append key value		将给定的 value 追加到原值的末尾
   strlen key			获得值的长度
   setnx key value			只有在 key 不存在时，设置 key 的值
   incr/decr key			将 key 中存储的数字增加/减少 1
   incrby/decrby key len		将 key 中存储的数字值增减，自定义步长
   mset key1 value...		同时设置一个或多个 key-value 对
   mget key1 key2...		同时获取一个或多个 value
   msetnx key1 value...		同时设置一个或多个 key-value 对，当且仅当所有给定 key 不存在
   getrange key start end		获得值的范围
   setrange key pos value		从 pos 开始用 value 复写 key 所存储的字符串值
   setex key time value		设置键值的同时，设置过期时间，单位秒
   getset key value		以新换旧，设置新值的同时获得旧值
   ```

#### <strong>List 列表类型</strong>

List 的底层实际是双向链表，对两端的操作性能较高，但是查询索引性能较差，

数据结构为快速链表（**quickList**），列表元素较少的情况下会使用一块连续的内存存储，

这个结构称为压缩列表 **（zipList）**，当数据量较多时，多个压缩列表组成快速链表，

简单的字符串列表，按照插入顺序排序，可以在列表首尾添加元素

```sql
lpush/rpush key value...	在左/右边插入一个或多值
lpop/rpop key			从左/右边弹出一个值，值在键在，值光键亡
rpoplpush key1 key2		从 key1 列表右边弹出一个值查到 key2 列表左边
lrange key start end		按照索引下标获得元素（从左到右）
lrange key 0 -1			获取所有元素
lindex key index		按照索引下标获得元素（从左到右）
llen key			获得列表长度
linsert key before v1 v2	在 v1 的前面插入 v2
lrem key n value		从左边删除 n 个 value
lset key index value		将列表 key 下标为 index 的只替换为 value
```

#### <strong>Set 集合类型</strong>

Set 提供的功能整体与 List 类似，特殊之处在于 Set 可以**自动排重**，

并且提供了判断某个成员是否在一个集合内的接口，底层无序集合，hash 表实现，

所以添加、删除、查找的时间复杂度都是 O（1），**数据结构是 dict 字典**

```sql
sadd key value...		将一个或多个元素不重复地加入到集合
smembers key			取出该集合地所有值
sismember key value		判断集合 key 是否含有该 value 值,有 1 无 0
scard key			返回该集合地元素个数
srem key value...		删除集合中一个或多个元素
spop key			从该集合中随机弹出一个元素
srandmember key n		从该集合中随机取出 n 个元素
smove src des value		将集合中地某个元素移动到另一个集合
sinter key1 key2		返回两个集合的交集元素
sunion key1 key2		返回两个集合的并集元素
sdiff key1 key2			返回两个集合的差集元素
```

#### <strong>Hash 哈希类型</strong>

键值对集合，String 类型的 field 和 value 的映射表，适合用于存储对象

```sql
hset key field value		给 key 中的 field 键赋值 value
hget key field 			从 key 中取出 field 的 value
hmset key field value...	批量设置 hash 值
hexists key filed		查看哈希表中，是否存在键 field
hkeys key			列出该 hash 集合中所有 field
hvals key			列出该 hash 集合中所有 value
hincrby key field inc		为 hash 中 field 的值上增量 1/-1
hsetnx key field value		当且仅当不存在设置键值对
```

#### <strong>Zset 有序集合</strong>

Zset 与普通集合大体相似，不同之处在于有序集合的每个成员关联一个**评分（score）**，

评分被用来按照从低到高的方式排序集合中的成员，成员唯一，但是评分可以重复，

SortedSet（Zset）是一个非常特别的数据结构，**底层使用两个数据结构：hash 和 跳跃表**

```sql
zadd key score value...		将一个或多个元素及其 score 值加入到有序集中
zrange key start stop		返回下标在 start 和 pop 之间的元素
zrangebyscore key min max	返回 score 值介于 min 和 max 之间的元素
zincrby	key inc value		为元素的 score 加上增量
zrem key value			删除指定元素
zcount key min max		统计分数区间内的元素个数
zrank key value			返回该值在集合中的排名
```

