---
title: 基本概念
date: 2021-11-29
updated: 2021-12-01
categories:
- Java
- Java基础知识
tags:
- Java
---

<escape><!--more--></escape>

### Java语言有哪些优点？

1. Java为纯面向对象的语言

2. 平台无关性

3. Java提供了很多内置的类库

4. 提供了对Web应用开发的支持

5. 具有较好的安全性和健壮性

6. 去除了C++语言中难以理解、容易混淆的特性

   **说明：Java语言是由C++语言改进并重新设计而来的**

----

### Java与C、C++有什么异同

1. **Java为半编译半解释型语言，而C / C++为编译型语言**

   * Java程序源代码先经过Java编译器编译成字节码，然后由JVM解释执行
   * C / C++程序源代码经过编译和链接后生成可执行的二进制代码

   因此C / C++的执行速度比Java快，但是Java因为JVM可以跨平台执行。

2. **Java为纯面向对象的语言，而C++兼具面向过程和面向对象的特点**

   * Java中除了基本数据类型，所有类型都是类
   * Java语言中不存在全局变量或者全局函数

3. **Java没有多重继承，但是可以实现多个接口**

4. **Java语言提供了垃圾回收器来实现垃圾的自动回收**

   * C++语言中，需要开发人员去管理对内存的分配（申请与释放）
   * Java的GC机制，不需要开发人员去关心内存空间

5. **Java具有平台无关性**

   + Java中每种基本类型数据都分配固定长度
   + C / C++中在不同的平台会分配不同的字节数

6. **其他不同的地方**

   1. Java不支持运算符重载
   2. Java没有预处理器，但是提供 import 机制
   3. Java不支持默认函数参数
   4. Java不提供 goto 语句，但是做为保留关键字
   5. Java不提供自动强制类型转化，需要显示转换
   6. Java不包含结构和联合，所有内容封装在类里面
   7. Java没有指针的概念，避免指针引起的系统问题
   8. Java提供了一些标准库，用于完成特定的任务，比如 JDBC

**常见的一个错误说法：Java语言中的方法属于类的成员**

静态方法：类成员

非静态方法：实例成员

----

### 为什么需要 main 方法

`public static void main(String args[]) {}`

1. 该方法为Java程序的入口方法，JVM在运行程序时，会首先查找 main() 方法

2. 字符串数组 args 为开发人员在命令行状态下与程序交互提供了一种手段

3. 因为 main() 方法是程序的入口，要执行一个类的方法，就必须实例化一个对象，

   此时还没有实例化对象，所以该方法需要被定义成 public 和 static 

4. main() 方法其他可用的定义格式：

   ***说明：public 和 static 的位置可以互换，没有先后关系***

   1. `static public void main(String args[]) {}`
   2. `public static final void main(String args[]) {}`
   3. `public static synchronized void main(String args[]) {}`

   **但是不可以用 abstract 关键字修饰，因为该函数为程序的入口方法**

----

### 一个Java文件中是否可以定义多个类

1. 可以定义多个类，但是**最多只能有一个被 public 修饰的类**，并且该类与文件名相同
2. 如果没有 public 类，那么文件名随便一个类的名字即可
3. 当编译此文件时，每一个类都会生成一个对应的字节码文件 .class
4. **普通方法可以与构造函数有相同的方法名**

----

### Java程序初始化的顺序

在Java语言中，当实例化对象时，对象所在类的所有成员变量首先要进行初始化，

只有当所有类成员完成初始化后，才会调用对象所在类的构造函数创建对象。

**程序初始化一般遵循三个原则（优先级依次递减）：**

1. 静态对象（变量）优先于非静态
2. 父类优先于子类
3. 按照成员变量的定义顺序进行初始化

**具体的执行顺序如下：**

父类静态变量、父类静态代码块、子类静态变量、子类静态代码块、

父类非静态变量、父类非静态代码块、父类构造函数、

子类非静态变量、子类非静态代码块、子类构造函数。

**示例代码如下：**

```java
class B extends Object {
    static {
        System.out.println("Load B");
    }

    public B() {
        System.out.println("Create B");
    }

    static {
        System.out.println("Load B");
    }
}

class A extends B {
    static {
        System.out.println("Load A");
    }

    public A() {
        System.out.println("Create A");
    }
}

public class TestClass {
    public static void main(String[] args) {
        new A();
    }
}
```

**程序运行结果是：**Load B	Load B	Load A	Create B	Create A

-----

### Java中的作用域

| 作用域与可见性 | 当前类 | 同一package | 子类 | 其他package |
| :------------: | :----: | :---------: | :--: | :---------: |
|     public     |   √    |      √      |  √   |      √      |
|   protected    |   √    |      √      |  √   |      ×      |
|    default     |   √    |      √      |  ×   |      ×      |
|    private     |   √    |      ×      |  ×   |      ×      |

注意：private 和 protected 不能用来修饰类

----

### Java中的标识接口

标识接口：没有任何方法声明的接口

作用：仅仅充当一个标识作用，用来表明实现它的类属于一个特定的类型

-----

### Java中的clone方法

实际编程中，需要从某个已有的对象 A 创建处另外一个与 A 具有相同状态的对象 B，

并且对 B 的修改不会影响到 A 的自身状态，因此提供了 clone 方法返回一个新的对象。

**使用clone方法的步骤：**

1. 实现clone的类首先需要继承 Cloneable 接口
2. 在类中重写 Object 类中的 clone() 方法
3. 在 clone() 方法章调用 super.clone() **（浅复制）**
4. 对对象中的非基本类型的属性也调用 clone() 方法 **（深复制）**
5. 把复制的引用指向原型对象新的克隆体

**示例代码如下：**

```java
import java.util.Date;

class Obj implements Cloneable {
    private Date birth = new Date();

    public Date getBirth() {
        return birth;
    }

    public void changeDate() {
        this.birth.setMonth(4);
    }

    @Override
    public Object clone() {
        Obj o = null;
        try {
            // 实现浅复制
            o = (Obj) super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        // 实现深复制
        o.birth = (Date) this.getBirth().clone();
        return o;
    }
}

public class TestClass {
    public static void main(String[] args) {
        Obj a = new Obj();
        Obj b = (Obj)a.clone();
        b.changeDate();
        System.out.println("a = " + a.getBirth());
        System.out.println("b = " + b.getBirth());
    }
}
```

**程序运行结果是：**

a = Mon Nov 29 19:05:03 CST 2021
b = Sat May 29 19:05:03 CST 2021

------

### Java创建对象的四种方式

1. 通过 new 语句实例化一个对象
2. 通过反射机制创建对象
3. 通过 clone() 方法创建一个对象
4. 通过反序列化的方式船舰一个对象