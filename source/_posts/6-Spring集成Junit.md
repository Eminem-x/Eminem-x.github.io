---
title: Spring集成Junit
categories:
- Java
- Spring
tags:
- Java
- Spring
---

## Spring集成Junit

<!--more-->

### 1. 原始 Junit 测试 Spring 的问题

在测试类中，每个测试方法都有一下两行代码：

```java
ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
IAccountService as = ac.getBean("accountService", IAccountService.class);
```

### 2. 解决办法

* 让 SpringJunit 负责创建 Spring 容器，但是需要将配置文件的名称告之；
* 将需要进行测试 Bean 直接在测试类中进行注入；

### 3. Spring 集成 Junit 步骤

1. 导入 Spring 集成 Junit 的坐标
2. 使用 @RunWith 注解替换原来的运行期
3. 使用 @ContextConfiguration 指定配置文件或配置类
4. 使用 @Autowired 注入需要测试的对象
5. 创建测试方法进行测试