---
title: Web 监听器
date: 2017-6-2 22:05:25
categories: [笔记]
tags: [Java, Web]
---

# Web 监听器

# 分类

## 监听对象分类

* 用于监听应用程序环境对象（ServletContext）的事件监听器
* 用于监听用户会话对象（HttpSession）的事件监听器
* 用于监听请求消息对象（ServletRequest）的事件监听器

### 监听的事件划分

* 监听域对象自身的创建和销毁的事件监听器
  * ServletContext
  * HttpSession
  * ServletRequest
* 监听域对象中的属性的增加和删除的事件监听器
  * ServletContext
    * ServletContextAttributeListener
  * HttpSession
    * HttpSessionAttributeListener
  * ServletRequest
    * ServletRequestAttributeListener
* 监听绑定到 HttpSession 域中的某个对象的状态的事件监听器
  * 略过

---

很粗略，但是学完了，也动手写了，还行