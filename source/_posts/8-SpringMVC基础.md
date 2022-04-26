---
title: SpringMVC基础
categories:
- Java
- Spring
tags:
- Java
- Spring
---

## SpringMVC基础

<!--more-->

### 1. SpringMVC概述

SpringMVC 是一种基于 Java 的实现 MVC设计模型的请求驱动类型的轻量级 Web 框架

1. 前端控制器：DispatcherServlet
2. 处理器映射器：HandlerMapping
3. 处理器适配器：HandlerAdapter
4. 处理器：Handler
5. 视图解析器：ViewResolver
6. 视图：View

### 2. SpringMVC步骤

需求：客户端发起请求，服务器端接受请求，执行逻辑并进行视图跳转

1. 导入 SpringMVC 相关坐标
2. 配置 SpringMVC 核心控制器 DispatcherServlet
3. 创建 Controller 类和视图页面
4. 使用注解配置 Controller 类中业务方法的映射地址
5. 配置 SpringMVC 核心文件 spring-mvc.xml
6. 客户端发起请求测试

### 3. SpringMVC执行流程

1. 用户发送请求至前端控制器 DispatcherServlet

2. DispatcherServlet 收到请求调用 HandlerMapping 处理器映射器

3. 处理器映射器找到具体的处理器，生成处理器对象以及处理器拦截器，

   一并返回给 DispatcherServlet

4. DispatcherServlet 调用 HandlerAdapter 处理器适配器

5. HandlerAdapter 经过适配器调用具体的处理器

6. Controller 执行完成后返回 ModelAndView

7. HandlerAdapter 将 ModelAndView 返回给 DispatcherServlet

8. DispatcherServlet 将 ModelAndView 传给 ViewReslover 视图解析器

9. ViewReslover 解析后返回具体 View

10. DispatcherServlet 根据 View 进行渲染视图后响应用户

### 4. SpringMVC注解解析

1. **@RequestMapping**

作用：用于建立请求 URL 和 处理请求方法之间的对应关系

位置：

* 类上，请求 URL 的第一级访问目录
* 方法上，请求 URL 的第二级访问目录

属性：

* value：用于指定请求的 URL
* method：用于指定请求的方式
* params：用于指定限制请求参数的条件，支持简单的表达式

### 5. SpringMVC的XML配置解析

1. mvc 命名空间引入和组件扫描
2. 配置内部资源视图解析器

