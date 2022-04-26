---
title: Spring与Web环境集成
categories:
- Java
- Spring
tags:
- Java
- Spring
---

## Spring与Web环境集成

<!--more-->

### 1. ApplicationContext应用上下文获取方式

应用上下文对象是通过 `new ClassPathXmlApplicationContext(Spring配置文件)`方式获取，

但是每次从容器中获得 Bean 时都要编写，会导致配置文件加载多次，应用上下文创建多次。



在 Web 项目中，可以使用 `ServletContextListener` 监听 Web 应用的启动，当应用启动时，

就加载 Spring 的配置文件，创建应用上下文对象，并将其存储到 `servletContext` 域中，

这样就可以在任意位置从域中获得应用上下文对象 `ApplicationContext` 对象。

### 2. Spring提供获取应用上下文的工具

Spring 提供监听器 `ContextLoaderListener` 对上述功能封装，

该监听器内部加载 Spring 配置文件，创建应用上下文对象，并存储到 `servletContext` 域中，

提供了一个客户端工具 `WebApplicationContextUtils` 供获得应用上下文对象。

1. 在 web.xml 中配置 ContextLoaderListener 监听器**（导入 spring-web 坐标）**
2. 使用 WebApplicationContextUtils 获得应用上下文对象

