---
title: Hibernate 一个实体类对应多张映射表相关问题
date: 2017-06-15 13:15:00
categories: [笔记] 
tags: [Java,Hibernate,SQL]
---

# 建表

在`.hbm.xml`文件中的`class`节点有 `entity-name`这条属性，就是当一个实体类需要映射多张表的时候用到的节点，相当于标记位吧

# 存储

Session 的 save 方法有相应的接口，传入相应的`entity-name`就可以

# 查询

这块迷惑我挺久的，其实很简单，`from`后面本来跟的是实体类的名字，现在改为`entity-name`即可