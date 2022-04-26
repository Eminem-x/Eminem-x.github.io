---
title: SpringMVC数据响应
categories:
- Java
- Spring
tags:
- Java
- Spring
---

## SpringMVC数据响应

<!--more-->

### 1. 页面跳转

1. ##### 直接返回字符串

此种方式会将返回的字符串与视图解析器的前后缀拼接后跳转

2. ##### 通过 ModelAndView 对象返回

### 2. 回写数据

1. ##### 直接返回字符串 / JSON

   * 通过 SpringMVC 框架注入的 response 对象，此时不需要视图跳转，方法返回值为 void
   * 将需要回写的字符串直接返回，通过 **@ResponseBody** 注解告知 SpringMVC 框架

2. ##### 返回对象或集合

通过 SpringMVC 进行 JSON 字符串的转换并回写，为处理器适配器配置消息转化参数，

指定使用 jackson 进行转换，因此需要在 spring-mvc.xml 中进行如下配置：

```xml
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
    <property name="messageConverters">
        <list>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
        </list>
    </property>
</bean>
```

在方法上添加 @ResponseBody 就可以返回 json 格式的字符串，但是配置比较繁琐，

因此，可以使用**注解驱动代替上述配置：`<mvc:annotation-driven/>`**

**处理器映射器、处理器适配器、视图解析器成为 SpringMVC 的三大组件**，

上述注解驱动默认底层就会集成 jackson 进行对象或集合的 json格式字符串的转换。

