---
title: SpringJdbcTemplate
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

### JdbcTemplate概述

Spring 框架中提供的对象，对原始繁琐的 Jdbc API 对象简单封装，提供了很多的操作模板类

### JdbcTemplate开发步骤

* 导入 spring-jdbc 和 spring-tx 坐标

  ```xml
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.0.5.RELEASE</version>
  </dependency>
  <dependency>
  <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>5.0.5.RELEASE</version>
  </dependency>
  ```

* 创建数据库表和实体

* 创建 JdbcTemplate对象

* 执行数据库操作

