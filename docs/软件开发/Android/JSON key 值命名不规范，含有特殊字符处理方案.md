---
title: JSON key 值命名不规范，含有特殊字符处理方案
date: 2017-3-10 14:05:25
categories: [笔记]
tags: [杂谈]
---

# JSON key 值命名不规范，含有特殊字符处理方案

json是有格式的字符串，键值对的形式存在。

有时候会碰到 json key 值命名不规范，有特殊符号在内。

用 JsonObject 来解析自然没什么问题，但是如果用 Gson 来解析到对象的话，就出大问题了。因为变量的命名是不可以包含这些特殊符号的。

Google 大神有办法。

Gson 有个注解

```java
@SerializeName("key")
```

这个注解可以将变量名和 key 值联系起来。

如：

```java
@SerializeName("key-1")
public getKey() {
  return mKey;
}
```



