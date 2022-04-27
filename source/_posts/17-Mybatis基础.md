---
title: Mybatis基础
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

### 原始 Jdbc 操作分析

**存在以下问题：**

1. 数据库连接创建、释放频繁造成系统资源浪费从而影响性能
2. sql 语句在代码中硬编码，不易维护，不便于实际应用
3. 查询和插入操作时，需要手动处理结果集和实体集

**解决方案：**

1. 使用数据库连接池初始化连接资源
2. 将 sql 语句抽取到 xml 配置文件中
3. 使用反射、内省等底层技术，自动将实体与表进行映射

### Mybatis 概念

* 基于 Java 的持久层框架，内部封装了 Jdbc，简化了开发者的工作
* 通过 xml 或注解的方式将要执行的各种 statement 配置起来
* 将结果映射为对象并返回，采用 ORM 思想解决实体和数据库映射问题
* 对 Jdbc 进行封装，屏蔽了底层访问细节，便于操作和维护

### 开发步骤

1. 添加 MyBatis 坐标

   ```xml
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>8.0.25</version>
   </dependency>
   <dependency>
       <groupId>org.mybatis</groupId>
       <artifactId>mybatis</artifactId>
       <version>3.4.6</version>
   </dependency>
   ```

2. 创建数据表（user）

3. 创建实体类（User）

4. 编写映射文件（UserMapper.xml）

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
   "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   
   <mapper namespace="userMapper">
       <select id="findAll" resultType="com.domain.User">
           select * from user
       </select>
   </mapper>
   ```
	* \<if>：if 判断
	* \<foreach>：循环
	* \<sql>：sql 片段抽取

5. 编写核心文件（SqlMapConfig.xml）

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" 
   "http://mybatis.org/dtd/mybatis-3-config.dtd">
   
   <configuration>
       <!--数据源环境-->
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC"></transactionManager>
               <dataSource type="POOLED">
                   <property name="driver" value="com.mysql.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://localhost:3306/study"/>
                   <property name="username" value="root"/>
                   <property name="password" value="123456"/>
               </dataSource>
           </environment>
       </environments>
   </configuration>
   ```

6. 编写测试类

   **操作涉及数据库数据变化时，要提交事务，即`sqlSession.commit()`**

### 核心配置文件

* environments 标签：数据源环境配置标签
* mappers 标签：加载映射配置
* properties 标签：加载外部的 properties 文件
* typeAliases 标签：设置类型别名
* typeHandlers 标签：重写或创建类型处理器来处理不支持或非标准的类型
* plugins 标签：Mybatis 可以使用第三方插件对功能进行拓展

### 相应API

1. #### SqlSession 工厂构建器 SqlSessionFactoryBuilder

   ```java
   // 获得核心配置文件
   InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
   // 获得session工厂对象
   SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
   ```

   通过加载核心文件输入流的形式构建一个 SqlSessionFactory 对象

2. #### SqlSession 工厂对象 SqlSessionFactory

   存在多个方法创建 SqlSession 实例，常用的如下两种：

   | 方法                            | 解释                                                         |
   | ------------------------------- | ------------------------------------------------------------ |
   | openSession()                   | 默认开启一个事务，但事务不会自动提交，需要手动提交，<br />更新数据才会持久化到数据库中 |
   | openSession(boolean autoCommit) | 参数为是否自动提交，如果为 true，那么不需要手动提交          |

3. #### SqlSession 会话对象

   包含所有执行语句、提交或回滚事务和获取映射器实例的方法

### Dao层代理开发

Mybatis的代理开发方式实现 DAO 层的开发：**Mapper接口开发方法**，

**只需要实现 Mapper 接口，由 Mybatis 框架根据接口定义创建接口的动态代理对象。**

**遵循以下开发规范：**

* Mapper.xml 文件中的 namespace 与 mapper 接口的全限定名相同
* Mapper 接口方法名和 Mapper.xml 中定义的每个 statement 的 id 相同
* Mapper 接口方法的输入参数类型和文件中定义的每个 sql 的 parameterType 类型相同
* Mapper 接口方法的输出参数类型和文件中定义的每个 sql 的 resultType 类型相同、

### MyBatis注解开发
