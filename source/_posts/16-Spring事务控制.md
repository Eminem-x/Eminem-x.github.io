---
title: Spring事务控制
categories:
- Java
- Spring
tags:
- Java
- Spring
---

## Spring事务控制

<!--more-->

### 1. 编程式事务控制相关对象

1. **PlatformTransactionManager** 是接口类型，不同的 Dao 层技术有不同的实现类，

   Jdbc 或 Mybatis 时：

   `org.springframework.jdbc.datasource.DataSourceTransactionManager`

   Hibernate 时：

   `org.springframework.orm.hibernate5.HibernateTransactionManager`

2. **TransactionDefinition** 是事务的定义信息对象

   1. ##### 事务隔离级别

      * ISOLATION_DEFAULT
      * ISOLATION_READ_UNCOMMITTED
      * ISOLATION_READ_COMMITTED
      * ISOLATION_REPEATABLE_READ
      * ISOLATION_SERIALIZABLE

   2. ##### 事务传播行为

      <strong>指的是当一个事务方法被另一个事务方法调用时，这个事务方法应该如何进行。 </strong>
      例如：A 事务方法调用 B 事务方法时，B 是继续在调用者 A 的事务中运行，

      还是为自己开启一个新事务运行，这就是由 B 的事务传播行为决定的。

   3. ##### 事务运行状态 TransactionStatus

### 2. 声明式事务控制

1. ##### 概念：

Spring 在配置文件中声明的方式来处理事务，代替代码式的处理事务

2. ##### 作用：

* 事务管理不侵入开发的组件，具体来说，业务逻辑对象就不会意识到正在事务管理之中，

  事实上也应如此，因为事务管理是属于系统层面的服务，而不是业务逻辑的一部分，

  如果想要改变事务管理策划的化，也只需要在定义文件中重新配置即可

* 不需要事务管理的时候，只要再设定文件上修改，便于维护

3. ##### Spring声明式事务控制底层就是AOP

### 3. 基于XML文件配置

```xml
<!--    配置平台事务管理器-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>

<!--通知 事务的增强-->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <!--	代表切点方法的事务参数的配置-->
        <tx:method name="*"/>
    </tx:attributes>
</tx:advice>

<!--    配置事务的aop织入-->
<aop:config>
    <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.service.impl.*.*(..))"/>
</aop:config>
```

### 4. 基于注解

1. 使用 **@Transactional** 在需要进行事务控制的类或方法上修饰
2. 注解使用在类上，那么该类下的所有方法都使用同一套注解参数配置
3. 使用在方法上，不同的方法可以采用不同的事务参数配置
4. 配置事务注解驱动 `<tx:annotation-driver>`
