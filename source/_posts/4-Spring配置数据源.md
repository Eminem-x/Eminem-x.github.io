---
title: Spring配置数据源
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

### 数据源（连接池）的作用

* 提高程序性能
* 事先实例化数据源，初始化部分连接资源
* 使用连接资源时从数据源中获取
* 使用完毕后连接资源归还给数据源

常见的数据源：DBCP、C3P0、BoneCP、Druid等

### 数据源的开发步骤

1. 导入数据源的坐标和数据库驱动坐标
2. 创建数据源对象
3. 设置数据源的基本连接数据
4. 使用数据源获取连接资源和归还连接资源

### Spring 配置数据源

可以将 DataSource 的创建权交由 Spring 容器完成

#### 抽取 jdbc 配置文件

applicationContext.xml 加载 jdbc.properties 配置文件获得连接信息。

1. 引入 context 命名空间和约束路径：

   命名空间：

   ```xml
   xmlns:context="http://www.springframework.org/schema/context"
   ```

   约束路径：

   ```xml
   http://www.springframework.org/schema/context 
   http://www.springframework.org/schema/context/spring-context.xsd
   ```

2. Spring 容器加载 properties 文件

   ```xml
   <context:property-placeholder location="xx.properties"/>
   <property name="" value="${key}"></property>
   ```

   

