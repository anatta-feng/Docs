---
title: Class 接口
date: 2018-1-09 00:02:25
categories: [笔记]
tags: [Java, 反射]
---

# isAssignableFrom

是用来判断一个类Class1和另一个类Class2是否相同或Class1是 Class2的超类或接口。   

通常调用格式是   

`Class1.isAssignableFrom (Class2) `

调用者和参数都是   java.lang.Class   类型。   

# getResource(String path)

获取资源路径，返回`Enumeration<URL>`

##Class.getResource(String path)

* Path 不以 `/` 开头的时候，默认是从此类所在的包下取资源
* path 以`/`开头的时候，则是从 ClassPath 根下取资源

## ClassLoader.getResource(String path)

* path 不能以 `/` 结尾
* path 是从 ClassPath 根下获取

# Class2.isAnnotationPresent(Class1)

注解 class1是否作用于 class2