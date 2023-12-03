---
title: HQL 查询
date: 2017-06-15 12:15:00
categories: [笔记] 
tags: [Java,Hibernate,SQL]
---

# 了解

## HQL 定义

> Hibernate Query Language，Hibernate 查询语言



## HQL 语句形式

`SELECT…` **`FROM…`** `WHERE…` `GROUP BY…` `HAVING…` `ORDER BY…`

* Select
  * 指定查询结果中的对象和属性
  * 指定以何种数据类型返回
* From
  * 指定查询目标(依照配置的持久化类，及其属性)
* Where
  * 逻辑表达式
  * 设置查询条件
  * 限制查询返回结果的范围
* Group by
  * 分组查询字句
* Having
  * 对分组进行条件限制
* Order by
  * 指定查询结果中实例对象的排序


## 注意事项

* HQL 是面向对象的查询语言，对 Java 类与属性大小写敏感
* HQL 对关键字不区分大小写

# 查询准备

*  org.hibernate.Query 接口
* Query 示例的创建
* 执行查询

## org.hibernate.Query 接口

1. Query 接口定义有执行查询的方法
2. Query 接口支持方法链的编程风格，是的程序代码更为简洁
   1. 参数的动态设置

## Query 实例的创建

1. Session 的 `createQuery()` 方法创建 Query 实例
2. createQuery 方法包含一个 HQL 语句参数，`createQuery(hql)`

## Query 执行查询

1. Query 接口的 `list()` 方法执行 HQL 查询
2. `list()`方法返回结果数据为`java.util.List`，List 集合中存放复合查询条件的持久化对象

# 检索对象 From

## from 字句

1. HQL 语句最简形式
2. from 指定了 HQL 语句查询主体—持久化类及其属性

## from 字句中持久化类的引用

1. 不需要引入持久化类的全限定名，直接引入类名
2. auto-import(自动引入)缺省情况

## 别名的使用

1. 为被查询的类指定别名
2. 在 HQL 语句的其他部分通过别名引用该类
3. 别名命名习惯
   1. `from Test as test`

# 选择—select 字句

## 通过 Object[] 返回查询结果

什么都不指定，默认以这种形式返回

## 以 List 形式返回

1. select 子句中使用 `new list` 指定
   1. `select new list(name,id) from test;`

## 通过 Map 返回

1. select 子句使用 new map 指定
2. key 值为索引值，字符串类型

```java
String hql = "select new map(id,name) from Test";
Querry q = s.createQuery(hql);

List<Map> maps = q.list();
for(Map m:maps) {
  print("name " + m.get("1"));
}
```

## 通过自定义类型返回

1. 持久化类中定义对应的构造器
2. select 子句中调用定义的构造器

## 使用 distinct 返回不重复的查询结果

`select distinct name from Test`

# 限制—where 语句

## 比较运算

> 持久化类的属性与给定的条件作比较

1. 比较运算符：`=` `<>` `<` `>` `>=` `<=`
2. null 值判断 `is [not] null`

`from Test as t where t.id > 10`

`from Test as t where t.id is null`

`from Test as t where t.id = null`

## 范围运算

1. [not] in (列表)
2. [not] between 值1 and 值2

## 字符串模式匹配

1. like 关键字
2. 通配符%、_
   1. %:任意个字符
   2. _:一个字符

## 逻辑运算

1. and(逻辑与)、or(逻辑或)
2. not（逻辑非）

## 集合运算

1. is [not] empty 集合[不]为空，不包含任何元素
2. member of 元素属于集合

## 在 HQL 中使用+-x/运算符

 四则运算符可以在 where 子句和 select 子句中使用

## 查询单个对象（uniqueResult 方法）

1. Query 接口的 uniqueResult 方法
2. where 子句条件的设置

# Order by 子句

排序

## 使用 order by子句对查询结果排序

1. 升序 asc
2. 降序 desc

`from Test order by id asc`