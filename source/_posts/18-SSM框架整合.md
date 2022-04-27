---
title: SSM框架整合
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

### 项目地址

从 GitHub 上 Fork 的项目：https://github.com/liyifeng1994/ssm/tree/master/src

并且该项目有详细的整合流程：https://blog.csdn.net/qq598535550/article/details/51703190

但是因为是几年前的项目，许多包的版本过时，无法正常启动，所以重新构建了整个项目，

跟着原作者的流程完成，整体不会出现太难解决的问题，并且该项目并无具体 web实现，

仅通过 test 测试整体框架的完备性，适合学习框架的设计流程。

### 更改版本

1. 数据库驱动版本：要和自己的数据库版本保持一致

2. c3p0 版本：https://blog.csdn.net/Peng_Ze_Yu/article/details/81632828

   在 Maven 导入时，更改版本信息，否则报错

3. Spring 版本：5.0.5.RELEASE

4. junit 版本：4.12 避免低版本不支持

   mybatis 以及 mybatis-spring 版本更新

5. entity：https://blog.csdn.net/qq_35991056/article/details/81390551

   entity / domain / model 的区别，在以后项目中应该也会遇到

6. 数据库的 appoint_time 修改为 appoint_date

   因为创建变量时，误写为 appointDate，所以更改了数据库列名

### 整合流程

1. 建立整个项目的基础目录结构，同时遵循 maven 的目录规范
2. 导入相应的 jar 包，资源坐标，注意版本号兼容性
3. 资源文件中建立 spring 文件，按照 dao、service、controller分层配置
4. 配置 mybatis 的核心文件 `mybatis-config.xml`
5. 配置 `web.xml` 文件
6. 根据需求，配置日志文件

3. 需要根据需求设计数据库以及查询语句

4. 在 entity 层中封装数据库实体，注意内部成员名称

5. 在 dao 层建立相对应的接口，由于 mybatis 不需要实现
6. 资源文件中编写相对应的 mapper 配置文件
7. 定义业务的数据字典，返回给客户端的状态信息
8. 在 dto 包下建立类存储执行操作后的返回结果
9. 在 dto 包下建立封装 json 返回结果的类，设计成泛型
10. 在 exception 下建立自定义异常类
11. 在 service 层下编写具体业务代码，创建并实现接口
12. 编写 controller 层即 web 层，完善整体结构

