---
title: SpringMVC拦截器
categories:
- Java
- Spring
tags:
- Java
- Spring
---

## SpringMVC拦截器

<!--more-->

### 1. 拦截器作用

SpringMVC 的拦截器类似于 Servlet 开发中的过滤器 Filter，用于对处理器进行**预处理**和**后处理**；

将拦截器按一定顺序连接成一条链，这条链成为**拦截器链（Interceptor Chain）**；

在访问被拦截的方法或字段时，拦截器链中的拦截器就会按之前定义的顺序被调用；

拦截器也是 **AOP** 思想的具体实现。

### 2. 拦截器和过滤器区别

| 区别     | 过滤器（Filter）                                             | 拦截器（Interceptor）                                        |
| -------- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| 适用范围 | servlet 规范中的一部分，<br />任何 Java Web 工程都可以使用   | SpringMVC框架才能使用                                        |
| 拦截范围 | 在 url-pattern 中配置 /* 后，<br />可以对所有要访问的资源拦截 | 在 \<mvc:mapping path="" /> 中配置 /** 后，也可以全部拦截，<br />但是可以通过  \<mvc:exclude-mapping path="" />标签，<br />排除不需要拦截的资源 |

### 3. 自定义拦截器

1. 创建拦截器类实现 HandlerInterceptor 接口

   | 方法名            | 说明                                                         |
   | ----------------- | ------------------------------------------------------------ |
   | preHandle()       | 方法将在请求处理之前进行调用，返回值为布尔类型，<br />当返回值为 false 时，表示请求结束；<br />当返回值为 true 时，继续调用下一个拦截器的 preHandle 方法 |
   | postHandle()      | 方法在 preHandle 方法返回值为 true 时才能被调用，<br />并且会在视图返回渲染之前被调用 |
   | afterCompletion() | 在整个请求结束后即渲染视图之后执行                           |

2. 配置拦截器在 spring-mvc.xml 文件中

   ```xml
   <!--    配置拦截器-->
   <mvc:interceptors>
       <mvc:interceptor>
           <!--    对哪些资源执行拦截操作-->
           <mvc:mapping path="/**"/>
           <bean class="com.yuanhao.controller.interceptor.MyInterceptor1"/>
       </mvc:interceptor>
   </mvc:interceptors>
   ```

3. 测试拦截器

