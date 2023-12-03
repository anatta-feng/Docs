---
title: web.xml 节点笔记
date: 2017-6-2 23:05:25
categories: [笔记]
tags: [Java, Web]
---

# Listener

```xml
<listener>
	<listener-class>...</listener-class>
</listener>
```

# Servlet

Servlet 配置

```xml
<servlet>
  <servlet-name>...</servlet-name>
  <servlet-class>...</servlet-class>
</servlet>
```

# Serlver-mapping

Servlet 对应配置

```xml
<servlet-mapping>
  <servlet-name>...</servlet-name>
  <url-pattern>...</url-pattern>
</servlet-mapping>
```

# Context-param

Application 初始化参数

```xml
<context-param>
  <param-name>...</param-name>
  <param-value>...</param-value>
</context-param>
```

# Session-config

Session 配置

```xml
<session-config>
  <!--session 超时-->
  <session-timeout>...</session-timeout>
  ...
</session-config>
```

# Filter

过滤器

```xml
<filter>
  <!--Filter 的名字-->
  <filter-name>...</filter-name>
  <filter-class>...</filter-class>
  <description>...</description>
  <init-param>
    <param-name>...</param-name>
    <param-value>...</param-value>
  </init-param>
</filter>

<filter-mapping>
  <filter-name>...</filter-name>
  <url-pattern>URL</url-pattern>
  <servlet-name>...</servlet-name>
  <!--可以是零对或多对，值为：REQUEST|INCLUDE|FORWARD|ERROR
      未设置的时候为 REQUEST-->
  <dispatcher>...</dispatcher>
</filter-mapping>
```



---



待续