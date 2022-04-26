---
title: Spring注解开发
categories:
- Java
- Spring
tags:
- Java
- Spring
---

## Spring注解开发

<!--more-->

Spring 是轻代码而重配置的框架，配置比较繁重，影响开发效率，

所以注解是一种开发趋势，注解代替 `xml` 配置文件可以简化配置，提高开发效率。

### 1. 原始注解

Spring 原始注解主要是替代 \<Bean> 的配置

| 注解           | 说明                                 |
| -------------- | :----------------------------------- |
| @Component     | 使用在类上实例化 Bean                |
| @Controller    | 使用在 web 层类上实例化 Bean         |
| @Service       | 使用在 service 层类上实例化 Bean     |
| @Repository    | 使用在 dao 层类上实例化 Bean         |
| @Autowired     | 使用在字段上用于根据类型依赖注入     |
| @Qualifier     | 结合 @Autowired 用于根据名称依赖注入 |
| @Resource      | 相当于 @Autowired + @Qualifier       |
| @Value         | 注入普通属性                         |
| @Scope         | 标注 Bean 的作用范围                 |
| @PostConstruct | 标注方法上表明是 Bean 的初始化方法   |
| @PreDestory    | 标注方法上表明是 Bean 的销毁方法     |

**注意：**

1. 使用注解进行开发时，需要在 `xml` 中配置组件扫描，其作用是：

   指定哪个包及其子包下的 `Bean` 需要进行扫描以便识别使用注解配置的类、字段和方法；

```xml
<context:component-scan base-package=""/>
```

2. `@Autowired` 根据数据类型从 `Spring` 容器中进行匹配

   `@Qualifier` 根据 `id` 值从容器中进行匹配，但是需要结合 `@Autowired` 使用；

3. 使用 `prototype` 时 `Spring` 不会负责销毁容器对象，将 `scope` 改为 `singleton` 才会生效 `destroy` 方法；

### 2. 新注解

* 非自定义的 `Bean` 配置：\<bean>
* 加载外部 `properties` 文件的配置：\<context:property-placeholder>
* 组件扫描的配置：\<context:component-scan>
* 引入其他文件：\<import>

| 注解            | 说明                                       |
| --------------- | ------------------------------------------ |
| @Configuration  | 用于指定当前类是一个 Spring 核心配置类     |
| @ComponentScan  | 用于指定 Spring 在初始化容器时要扫描的包   |
| @Bean           | 使用在方法上，将该方法的返回值存储到容器中 |
| @PropertySource | 用于加载 properties 文件                   |
| @Import         | 用于导入其他配置类                         |



