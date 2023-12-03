---
title: 2017
date: 2017-1-12 17:05:25
categories: [笔记]
tags: [Java]
---
# Java 执行 Shell 命令

> 在 Java 代码中执行 shell 命令，我想弄挺久的，最近刚好工作需要，便学习了番

## 执行 shell 命令

执行 shell 很简单，一行代码搞定

```java
Runtime.getRuntime().exec("command");
```

具体的实现逻辑大家感兴趣的可以去看看源码

---

## 获取 shell 执行结果

有时候我们需要获取到 shell 命令的执行结果，这时候只要稍微留点心就会发现```exec()```方法是有一个返回值的，这个值是 Progress 对象

```java
Progess pro = Runtime.getRuntime().exec("command");
BufferReader br = new BufferReader(new
  InputStreamReader(pro.getInputStream()));
String line = "";
List<String> list = new ArrayList<>();
while((line = br.readLine()) != null) {
  list.add(line);
}
br.close();
for(String line : list) {
  System.out.prinfln(line);
}
```

---

但是有的命令并没有返回结果，但是我们需要知道他是否执行完毕

```java
if(pro.waitFor() == 0) {
  System.out.println("success");
}
```

