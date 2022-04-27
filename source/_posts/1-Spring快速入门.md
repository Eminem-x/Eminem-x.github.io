---
title: Spring快速开始
date: 2022-01-31
updated: 2022-01-31
categories:
- Java
- Spring
tags:
- Java
- Spring
---

<escape><!--more--></escape>

**PS：仅作为个人学习笔记，以后深入学习再补充**

Spring 学习笔记地址：https://github.com/Eminem-x/Spring

SpringBoot学习笔记地址：https://github.com/Eminem-x/SpringBoot

推荐 Spring 学习视频：<a href="https://www.bilibili.com/video/BV1WZ4y1P7Bp?from=search&seid=9648668674109084850&spm_id_from=333.337.0.0">黑马程序员 SSM 框架教程</a>

推荐 SpringBoot 学习视频：<a href="https://www.bilibili.com/video/BV19K4y1L7MT?from=search&seid=14164871616704737958&spm_id_from=333.337.0.0">尚硅谷 雷神 SpringBoot 学习</a>

SpringBoot 学习参考：<a href="http://www.ityouknow.com/springboot/2016/01/06/spring-boot-quick-start.html">ityouknow-SpringBoot学习</a>

-----

### Spring程序开发步骤

1. 导入 Spring 开发的基本包坐标（通过Maven）；
2. 创建 Bean；
3. 创建 Spring 核心配置文件；
4. 在 Spring 配置文件中进行配置；
5. 使用 Spring 的 API 获得 Bean 实例；

### 基本概念

1. Dao 是数据访问层，Service 是业务层；

2. Dao 的作用是封装对数据库的访问：CRUD，不涉及业务逻辑；

3. Service 则专注业务逻辑，对于其中需要的数据库操作，通过 Dao 实现；

4. 这样的分层是**基于 MVC 架构**而言：

   M是指业务模型（model），V是指用户界面（view），C则是控制器（controller），

   使用MVC的目的是将M和V的实现代码分离，从而使同一个程序可以使用不同的表现形式。

5. 分层的作用是**解耦**；

6. **对于 Spring 框架：**

   （View\Web）表示层调用控制层（Controller），

   控制层调用业务层（Service），

   业务层调用数据访问层（Dao）。

7. **Bean 是一个被实例化，组装，并通过 Spring IoC 容器所管理的对象。**

