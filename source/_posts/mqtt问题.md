---
title: Mqtt机制问题
category: Tool
tags: Tool
---

<escape><!--more--></escape>

### Preface

最近在灰度测试过程中，业务侧报上来一个偶现问题，也是困扰了我一个周末，最终于周一解决：

> 广州用户在自助柜前，没有点击扫码出货，但是出货了，并且后台中没有该用户的记录

直觉告诉我，这绝不是偶现，并且我内心里很确定，这绝对是个漏洞，因为计算机不会骗人，所以苦想了很久，

功夫不负有心人，成功解决，并且由于新灰度测试城市增加，果然复现几率增加，过程中学到很多，记录如下。

-----

### Details

#### <strong>排查问题</strong>

因为后端项目最初没有接入 `LogID`，所以排查困难，不过由于几周前我接入了流氏日志，

所以可以通过 `Log` 、数据库记录和当时的操作人员回忆流程，去判断和定位问题原因，

经过排查，发现一条特殊的数据，同时间下，西安某个用户借用了此物品，梳理流程后，觉得问题应该在这，

<strong>但是后端通过 mqtt 推送的消息怎么会被两台机器同时消费呢？并且为什么其他地方没有出现这个问题呢？</strong>

带着疑问，我先排查后端代码问题，确认无误后，联系客户端同学，进过讨论，得到了答案：

<strong>客户端同学以为后端 mqtt 推送是指定了具体的 clientID，因此没有做检验，所以导致同时出货，</strong>

那么为什么其他地方没有出现呢？原因是：<strong>客户端只有在显示出货那个界面时，才会接收 mqtt 推送消息。</strong>

得到原因后，立刻和客户端同学模拟测试，果不其然就是因为这个原因造成的，但是对于如何解决出现了分歧。

#### <strong>解决方案</strong>

* 后端指定具体的 `clientID` 推送 mqtt 消息

* 客户端在接收 mqtt 消息时，做一次判断比较，确定是否消费

* 对于每台客户端机器根据 `machine_code` 生成唯一的 `topic`，确保推送唯一

#### <strong>查询资料</strong>

首先通过<a href="https://www.emqx.io/docs/en/v5.0/#benefits">官网</a>的介绍，可以得到以下内容，说明 mqtt 本身就是高性能的，一个实例足够撑起业务：

> ><strong>Massive Scale</strong>: Scale to 100 million concurrent MQTT connections with a single EMQX 5.0 cluster.
> >
> ><strong>High Performance</strong>: Move and process millions of MQTT messages per second in a single broker.
> >
> ><strong>Low Latency</strong>: Guarantee sub-millisecond latency in message delivery with the soft real-time runtime.

其次我的同事，通过 <a href="https://stackoverflow.com/questions/42265001/how-to-publish-a-message-to-a-specific-client-in-mosquitto-mqtt">stackoverflow </a>获得了一个关于 mqtt 指定客户端推送非常棒的回答：

> There is no way to publish a message to a single subscriber at the MQTT protocol level.
>
> One of the key tenets of a publish/subscribe system is to totally decouple the publisher from the subscribers, there is no way for a publisher to know if there are any subscribers to a given topic let alone target one specifically.
>
> Using a topic for each device is not a problem as there is virtually no overhead in the broker for each topic. You can also use ACLs to ensure that each client can only subscribe their own topic (while still being able to publish to others if needed)
>
> You could use a single topic that all clients subscribe to and encode the target device in the payload and have the device decide if the message is for it's self. One downside to this is that you can't apply ACLs to this model. Another downside is increased network traffic, since uninteresting messages will be sent to many subscribers.

<strong>通过以上资料可以确定当下最好的解决方案就是第二种：客户端根据自身的唯一标识判断是否消费当前指令，</strong>

确定好方案后，经过测试，解决问题，也是在解决不久后，业务侧开始大量反馈此问题，所以不要放过漏洞。

------

### Final

我非常庆幸我自己对于这个偶现现象的坚持，因为最开始的反馈是在上周五，周六我夜不能寐，放弃秋招笔试，

查询 mqtt 资料，查询数据库记录、查询后端日志、下载客户端代码、回忆情景，直到凌晨三点多，

尽管当时没有解决，但是大量的资料数据收集，让我能够确定问题出在哪里，并且联系客户端同学进行排查出问题所在，

我也非常庆幸我的坚持，让这个问题在增加灰度测试站点时，得以快速解决，而不是简单地认为不关自己的事情，随它去吧，

尽管我不一定在这个部门转正，尽管我还有秋招，但是我觉得重要的事情是，做好当下手头的工作，提升自己，而不是追逐，

也是通过这个事情，我更加觉得基础的重要性，因为应用上都大体相似，但是基础知识的积累可以更好的定位问题，

并且对于业务代码而言，经验也是非常重要的，也算是肯定了我不去读研，投身于具体工作的决心吧，希望以后继续坚持。