---
title: SpringMVC异常处理
categories:
- Java
- Spring
tags:
- Java
- Spring
---

## SpringMVC异常处理

<!--more-->

### 1. 异常处理的思路

系统中异常包括两类：预期异常和运行时异常；

前者通过捕获异常从而获取异常信息，后者通过代码规范、测试减少发生。

**系统的 Dao、Service、Controller出现都通过 throws Exception 向上抛出，**

**最后由 SpringMVC 前端控制器交由异常处理器进行异常处理。**

### 2. 异常处理的两种方式

* SpringMVC 提供的简单异常处理器：SimpleMappingExceptionResolver

  在使用时可以根据项目情况进行相应异常与视图的映射配置：

  ```xml
  <!--    配置简单映射异常处理器-->
  <bean
      class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
      <property name="defaultErrorView" value="error"/>
      <property name="exceptionMappings">
          <map>
              <entry key="com.yuanhao.exception.MyException" value="error"/>
              <entry key="java.lang.ClassCastException" value="error"/>
          </map>
      </property>
  </bean>
  ```

* Spring 的异常处理接口 HandleExceptionResolver 自定义异常处理器

  1. 创建异常处理器类实现接口

  2. 配置异常处理器

     ```xml
     <!--    自定义异常处理器-->
     <bean class="com.resolver.MyExceptionResolver"/>
     ```

  3. 编写异常页面

  4. 测试异常跳转

