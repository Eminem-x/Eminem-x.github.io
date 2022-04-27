---
title: Redis特性
date: 2022-01-31
updated: 2022-01-31
categories:
- Redis
tags:
- Redis
---

<escape><!--more--></escape>

### 配置文件

配置信息存储在 `redis.windows.conf` 文件

----

### 发布订阅

#### 1. 概念

发布订阅（pub / sub）是一种消息通信模式：发送者（pub）发送信息，订阅者（sub）接收信息，

Redis 客户端可以订阅任意数量的频道，发布的消息没有持久化。

#### 2. 命令行实现

* 打开一个客户端订阅 channel1：`subscribe channel1`

* 打开另一个客户端，给 channel1 发布消息：`public channel1 helloworld`

* 打开第一个客户端可以接收到发送的信息

----

### 新数据类型

#### 1. Bitmaps

合理地使用操作位能够有效地提高内存使用率和开发效率

Redis 提供 Bitmaps 实现对位的操作：

* 本身不是一种数据类型，实际上就是字符串，不过可以对位进行操作

* 提供单独的一套命令，可以堪称一个以位为单位的数组

  ```sql
  setbit key offset value		设置 Bitmaps 中某个偏移量的值
  getbit key offset		获取 Bitmaps 中某个偏移量的值
  bitcount key start end		统计字符串被设置为 1 的 bit 数
  bitop op dest src		多功能符合操作
  ```

#### 2. HyperLogLog

工作当中，会遇到**基数问题：**求集合中不重复元素的个数，解决方案：

1. 数据存储在 MySQL 中，使用 `distinct count` 计算不重复个数
2. 使用 Redis 提供的 hash、set、bitmaps 等数据结构来处理

以上方案的结果准确，但是随着数据规模增加，导致占用空间非常大，

**而 HyperLogLog 可以降低一定的精度来平衡存储空间，用来做基数统计。**

```sql
pfadd key element		添加指定元素到 HyperLogLog 中
pfcount key			计算 HLL 近似基数值
pfmerge dest src		将一个或多个 HLL 合并后的结果存在 dest 中 
```

#### 3. Geospatial

对 GEO 类型的支持，**GEO（Geographic）地理信息的缩写，二维坐标**

基于该类型，提供了经纬度设置、范围查询、距离查询等常见操作。

----

### 事务和锁机制

#### 1. 事务概念

Redis 事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行，

事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。

#### 2. 事务作用

Redis 事务的主要作用就是串联多个命令防止别的命令插队

#### 3. 操作命令

* Multi：输入的命令依次进入命令队列中
* Exec：将之前的命令队列中的命令依次执行
* Discard：放弃组队

#### 4. 事务特性

1. <strong>单独的隔离操作</strong>

   事务中的所有命令都会序列化、按顺序地执行，事务在执行的过程中，

   不会被其他客户端发送来的命令请求所打断

2. **没有隔离级别的概念**

   队列中的命令没有提交之前都不会实际被执行，

   因为事务提交前任何指令都不会被实际执行

3. **不保证原子性**

   事务中如果有一条命令执行失败，其后的命令仍然会被执行，不回滚



