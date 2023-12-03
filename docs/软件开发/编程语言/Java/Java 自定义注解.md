---
title: Java 自定义注解
date: 2017-5-16 23:05:25
categories: [笔记]
tags: [Java]
---

# 元注解

> 负责注解其他注解
>
> Java5.0 提供五个元注解

* @Target
* @Retention
* @Documented
* @Inherited

## @Target

@Target 注解说明了 Annotation 所修饰的对象范围：

Annotation 可被用于修饰 package，types（类、接口、枚举、Annotation 类型）、类型成员（方法、构造方法、成员变量、枚举值）、方法参数和本地变量。

在 Annotation 类型的生命中使用@target 可更加明确其要修饰的目标

**作用：用于描述注解的作用范围（被描述的注解可以在什么地方使用）**

取值：

1. CONSTRUCTOR:用于描述构造器
2. FIELD:用于描述变量、属性
3. LOCAL_VARIABLE:用于描述局部变量
4. METHOD:用于描述方法
5. PACKAGE:用于描述包
6. PARAMETER:用于描述参数
7. TYPE:用于描述类、接口等
8. ANNOTATION_TYPE:可用于注解类型上（被@interface修饰的类型）

---

## @Retention

@Retention 定义了该 Annotation 被保留的时间长短：

有的 Annotation 只存在于代码中，在编译过程中丢弃，有的却被编译在 class 文件中

编译中丢弃的注解不会对虚拟机造成影响，而编译进 class 文件的注解则会在 class 文件装载的时候被读取。

简而言之@Retention 是用来定义注解的生命周期的

取值：

1. SOURCE:在原文件中有效
2. CLASS:在 class 文件中有效
3. RUNTIME:在运行时有效，始终不会被丢弃，因此可以使用反射机制读取该注解的信息。自定义注解通常使用这种方式

---

## @Documented

表示是否将注解信息添加到 java 文档中

---

## @Inherited

定义了该注解和子类的关系

如果一个使用了@Inherited 修饰的注解被用于一个 class，那么这个注解就会被用于该 class 的子类

---

# 自定义注解

使用`@interface`声明注解，不能继承其他的注解或者接口。

其中的每一个方法实际上是声明了一个配置参数。方法的名字就是参数的名称，返回值的类型就是参数的类型（返回值类型只能是基本类型、Class、String、enum）。可以通过 default 来声明参数的默认值

**声明格式：**

```java
public @interface 注解名 {定义体}
```

**注解参数支持的数据类型：**

1. 所有基本数据类型
2. String
3. Class
4. enum
5. Annotation
6. 以上所有类型的数组

### 参数设置：

1. 只能用`public`或者默认`default`来修饰访问权限
2. 参数成员只能用基本类型和 String，Enum，Class，Annotation 等数据类型，以及这些类型的数组
3. 如果只有一个参数成员，最好把参数名称设为"value"后接小括号

### 注解元素的默认值

注解元素必须有确定值，要么在自定义注解的默认值中指定，要么在使用注解时指定，非基本类型的注解元素的值不可为 null。

一般都用0或者空字符串作为默认值，但是在注解中这样很难表示一个元素是否存在或者缺失，所以使用空字符串或者负数来表示某个元素不存在