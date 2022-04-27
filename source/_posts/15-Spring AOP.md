---
title: Spring AOP
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

### AOP的概念

AOP 为 Aspect Oriented Programming 的缩写，意味**面向切面编程**，

是通过**预编译方式和运行期动态代理**实现程序功能的统一维护的一种技术，

利用 AOP 可以对业务逻辑的各个部分进行隔离，从而使业务逻辑之间松耦合。

* Target（目标对象）：代理的目标对象
* Proxy（代理）：一个类被 AOP 织入增强后，就产生一个结果代理类
* Joinpoint（连接点）：可以被拦截到的点，在 Spring 中指的是方法
* Pointcut（切入点）：对哪些 Joinpoint 进行真正的拦截的定义
* Advice（通知/增强）：拦截到 Joinpoint 之后所要做的事情
* Aspect（切面）：切入点和通知的结合
* Weaving（织入）：把增强应用到目标对象来创建新的代理对象的过程

-----

### AOP的作用及其优势
作用：在程序运行期间，在不修改源码的情况下对方法进行功能增强

优势：减少重复代码，提高开发效率，并且便于维护

-----

### AOP的底层实现

通过 Spring 提供的动态代理技术实现，在程序运行期间，

Spring 通过动态代理技术动态的生成代理对象，代理对象方法执行时，

进行增强功能的介入，再去调用目标对象的方法，从而完成功能的增强。

-----

### AOP的动态代理技术

1. JDK 代理：基于接口的动态代理技术
2. cglib 代理：基于父类的动态代理技术

-----

### AOP的开发明确事项

1. **需要编写的内容**
   * 编写核心业务代码（目标类的目标方法）
   * 编写切面类、切面类中有通知（增强功能方法）
   * 在配置文件中，配置织入关系
2. **AOP技术实现的内容**

Spring框架监控切入点方法的执行，一旦监控到切入点方法被运行，使用代理机制，

动态创建目标对象的代理对象，根据通知类别，在代理对象的对应位置，

将通知对应的功能织入，完成完整的代码逻辑运行。

3. **AOP底层使用那种代理方式**

在Spring中，框架会根据目标类是否实现了接口来决定采用哪种动态代理方式

----

### 基于XML的AOP开发

1. **步骤：**

   1. 导入AOP相关坐标

      ````xml
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>5.0.5.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.aspectj</groupId>
          <artifactId>aspectjweaver</artifactId>
          <version>1.8.4</version>
      </dependency> 
      ````

   2. 创建目标接口和目标类（内部有切点）

   3. 创建切面类（内部有增强方法）

   4. 将目标类和切面类的对象创建全交给Spring

   5. 在 appliactionContext.xml 中配置织入关系

   6. 测试代码

2. **切点表达式的写法：**

   `execution([修饰符] 返回值类型 包名.类名.方法名(参数))`

   * 访问修饰符可以省略
   * 返回值类型、包名、类名、方法名可以使用星号 * 代表任意
   * 包名与类名之间两个点表示当前包及其子包下的类
   * 参数列表可以使用两个点表示任意个数，任意类型的参数列表

3. **通知类型：**

   `<aop:通知类型 method= ”切面类中方法名“ pointcut=”切点表达式“/>`

   | 名称         | 标签                   | 说明                               |
   | ------------ | ---------------------- | ---------------------------------- |
   | 前置通知     | \<aop:before>          | 指定增强方法在切入点方法之前执行   |
   | 后置通知     | \<aop:after-returning> | 指定增强方法在切入点方法之后执行   |
   | 环绕通知     | \<aop:round>           | 指定增强方法在切入点之前之后都执行 |
   | 异常抛出通知 | \<aop:throwing>        | 指定增强的方法在出现异常时执行     |
   | 最终通知     | \<aop:after>           | 无论增强方式执行是否有异常都会执行 |

-----

### 基于注解的AOP开发

1. **步骤：**

   1. 创建目标接口和目标类

   2. 创建切面类

   3. 将目标类和切面类的对象创建权交给 Spring

   4. 在切面类中使用注解配置织入关系

   5. 在配置文件中开启组件扫描和 AOP 的自动代理

      `<aop:aspectj-autoproxy/>`

   6. 测试代码

2. **切点表达式的抽取：**

   ````java
   @Before("MyAspect.pointcut()")
   public void before() {}
       
   @Pointcut("execution(* com.*.*(..))")
   public void pointcut(){}
   ````

   

