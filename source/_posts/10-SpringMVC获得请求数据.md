---
title: SpringMVC获得请求数据
categories:
- Java
- Spring
tags:
- Java
- Spring
---

## SpringMVC获得请求数据

<!--more-->

客户端请求参数的格式是：**name=value&name=value...**

服务器端要获得请求的参数，有时还需要进行数据的封装，

### 1. 接收参数类型

1. ##### 基本类型参数

   Controller 中的业务方法的参数名称要与请求参数一致，参数值会自动映射匹配

2. ##### POJO类型参数

   Controller 中的业务方法的 POJO 参数的属性名与请求参数一致，自动映射匹配

3. ##### 数组类型参数

   Controller 中的业务方法的 数组名称的与请求参数一致，自动映射匹配

4. ##### 集合类型参数

   1. 获得集合参数时，需要将集合参数包装到 POJO

   2. 使用 ajax 提交时，可以指定 contextType 为 json 形式，

      在方法参数位置使用 **@RequsetBody** 可以直接接收集合数据

### 2. **请求数据乱码问题：**

当 post 请求时，数据会出现乱码，设置过滤器进行编码的过滤

```xml
<!--    配置全局过滤器-->
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>GBK</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

### 3. 参数绑定注解

当请求与业务方法的参数名称不一致时，就需要通过 **@RequsetParam** 注解显示的绑定

* value：请求参数名称
* required：指定请求参数是否必须包括，默认值为 true
* defaultValue：当没有指定请求参数时，则使用默认值赋值

### 4. 获得Restful风格的参数

Restful 是一种软件架构风格、设计风格，而不是标准，

主要用于客户端和服务器交互类的软件，基于此风格设计，

可以使得软件更加简洁，具有层次，易于实现缓存机制。

**使用"url + 请求方式" 表示一次请求目的：**

* GET：用于获取资源
* POST：用于新建资源
* PUT：用于更新资源
* DELETE：用于删除资源

在方法中使用 **@PathVariable** 注解进行占位符的匹配获取。

### 5. 自定义类型转换器

默认例子：客户端提交的字符串转换成 int 型进行参数设置

现实需求：日期类型的数据就需要自定义类型转换器

**具体开发步骤：**

1. 定义转换器类实现 Converter 接口
2. 在配置文件中声明转换器
3. 在 \<annotation-driven> 中引用转换器

### 6. 获得Servlet相关API

SpringMVC支持使用原始ServletAPI对象作为控制器方法的参数进行注入：

* HttpServletRequest
* HttpServletResponse
* HttpSession

### 7. 获得请求头

1. #### @RequestHeader

注解属性如下：

* value：请求头的名称
* required：是否必须携带此请求头

2. #### @CookieValue

注解属性如下：

* value：指定 cookie 的名称
* required：是否必须携带此 cookie

### 8. 文件上传

1. #### 文件上传客户端三要素

   * 表单项 type = "file"
   * 表单的提交方式是 post
   * 表单的 enctype 属性是多部分表单形式

2. #### 单文件上传步骤

   * 导入 fileupload 和 io 坐标

     ```xml
     <dependency>
         <groupId>commons-fileupload</groupId>
         <artifactId>commons-fileupload</artifactId>
         <version>1.3.1</version>
     </dependency>
     <dependency>
         <groupId>commons-io</groupId>
         <artifactId>commons-io</artifactId>
         <version>2.3</version>
     </dependency>
     ```

   * 配置文件上传解析器

     ```xml
     <!--    配置文件上传解析器-->
     <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
         <property name="defaultEncoding" value="UTF-8"/>
         <property name="maxUploadSize" value="500000"/>
     </bean>
     ```

   * 编写文件上传代码

3. #### 多文件上传

   避免重复步骤，将方法参数 MultipartFile 类型修改为 MultipartFile[] 即可

