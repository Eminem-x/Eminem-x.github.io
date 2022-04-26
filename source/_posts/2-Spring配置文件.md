---
title: Spring配置文件
categories:
- Java
- Spring
tags:
- Java
- Spring
---

## Spring配置文件

<!--more-->

### 1. Bean 标签基本配置

1. Bean 用于配置对象交由 Spring 来创建，**默认情况下调用的是类中的无参构造函数**，

   如果没有无参构造函数则不能创建成功；

2. 基本属性

   1. **id**：Bean 实例在 Spring 容器中的唯一标识

   2. **class**：Bean 的全限定名称

   3. **scope**：Bean 的作用范围

   4. <strong>\<property></strong>标签：属性注入
      1. name 属性：属性名称
      2. value 属性：注入的普通属性值
      3. ref 属性：注入的对象引用值
      4. \<list>标签、\<map>标签、\<properties>标签
      
   5. **\<constructor-arg>标签**
   
3. **\<import>标签**：导入其他的 Spring 的分文件

### 2. Bean 标签范围配置

|    取值范围    |              说明              |
| :------------: | :----------------------------: |
|   singleton    |          默认值、单例          |
|   prototype    |              多例              |
|    request     |  WEB项目中，存入到 request 域  |
|    session     |  WEB项目中，存入到 session 域  |
| global session | WEB项目中，应用在 Portlet 环境 |

1. #### 当 scope 的取值为 singleton 时：

* Bean 的实例化个数：1个
* Bean 的实例化时机：当 Spring 核心文件被加载时，实例化配置的 Bean 实例
* Bean 的生命周期：
  1. 对象创建：当应用加载，创建容器时，对象就被创建；
  2. 对象运行：只要容器在，对象就一直存在；
  3. 对象销毁：当应用卸载，销毁容器时，对象就被销毁；

2. #### 当 scope 的取值为 prototype 时：

* Bean 的实例化个数：多个
* Bean 的实例化时机：当调用 getBean() 方法时实例化 Bean
* Bean 的生命周期：
  1. 对象创建：当使用对象时，创建新的对象实例；
  2. 对象运行：只要对象在使用中，就一直存在；
  3. 对象销毁：当对象长时间不用，就被 Java 的垃圾回收期回收；

### 3. Bean 生命周期配置

* init-method：指定类中的初始化方法名称

* destroy-method：指定类中的销毁方法名称

  **注意：需要手动关闭容器，才能运行销毁方法**

**特殊说明：**

当创建 ApplicationContext 对象时，不存在 close() 方法，有两种解决方法：

1. 更改为 AbstractApplicationContext 对象；
2. **向下强制转为 ConfigurableApplicationContext，即声明时的类型；**
3. 在大多数情况下更加推荐第二种方法；
4. 参考文章：[java - How to close a spring ApplicationContext? - Stack Overflow](https://stackoverflow.com/questions/14423980/how-to-close-a-spring-applicationcontext)

### 4. Bean 实例化三种方式

* 无参构造方法实例化
* 工厂静态方法实例化
* 工厂实例方法实例化

### 5. Bean 依赖注入

1. **依赖注入（Dependency Injection）: Spring框架核心 IOC 的具体实现**

2. 依赖注入方式：
   * 构造方法
   * set 方法

3. 依赖注入的数据类型：
   * 普通数据类型
   * 引用数据类型
   * 集合数据类型

### 6. 引入其他配置文件（分模块开发）

实际开发中，Spring 的配置内容非常多，导致配置很繁杂并且体积大，

所以可以将部分配置拆解到其他配置文件中，而在 Spring 主配置文件中，

通过 import 标签进行加载。