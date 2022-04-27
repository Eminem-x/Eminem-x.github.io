---
title: Spring相关API
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

各个版本的 API 文档链接：https://docs.spring.io/spring-framework/docs/

### ApplicationContext 的继承体系

ApplicationContext：接口类型，代表应用上下文，可以通过其实例获得 Spring 容器中的 Bean 对象

### ApplicationContext 的实现类

* **ClassPathXmlApplicationContext**

  从类的根路径下加载配置文件**（推荐使用）**

* **FileSystemXmlApplicationContext**

  从磁盘路径上加载配置文件，配置文件可以在磁盘上的任意位置

* **AnnotationConfigApplicationContext**

  当使用注解配置容器对象时，需要用此类创建容器，用来读取注解

### getBean() 方法

```java
java.lang.Object getBean(java.lang.String name) throws BeansException
<T> T getBean(java.lang.Class<T> requiredType) throws BeansException
```

1. 参数类型是字符串时，表示根据 Bean 的 id 从容器中获得实例，

   返回时 Object，需要进行强制类型转换

2. 参数类型是 Class 时，表示根据类型从容器中匹配 Bean 实例，

   不需要强制类型转换，但是当有多个相同 Bean 时，会报错

