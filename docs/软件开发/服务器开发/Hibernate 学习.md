---
title: Hibernate 学习
date: 2017-6-8 17:05:25
categories: [笔记]
tags: [数据库, Java, Hibernate]
---

# Hibernate 配置文件

需要配置5条属性

* connection.username
* connection.password
* connection.driver_class
  * 数据库驱动
* connection.url
  * 访问数据库的地址
* dialect
  * 方言

代码

```xml
<property name="connection.username">root</property>
<property name="connection.password">root</property>
<property name="connection.driver_class">com.mysql.jdbc.Driver</property>
<property name="connection.url">jdbc:mysql:///hibernate?userUnicode=true&amp;characterEncoding=UTF-8</property>
<property name="dialect">org.hibernate.dialect.MySQLDialect</property>
```

# 实体类的编写，以及实体类映射表

比如一个学生实体类

```java
package com.fxc.test.entity;

import javax.persistence.*;
import java.sql.Date;

/**
 * Describe :
 * Author : fxc.
 * Date : 2017/6/7.
 */
@Entity
@Table(name = "Students", schema = "Hibernate", catalog = "")
public class StudentsEntity {
	private int id;
	private int sid;
	private String name;
	private Date birthday;
	private String address;

	@Id
	@Column(name = "id")
	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	@Basic
	@Column(name = "sid")
	public int getSid() {
		return sid;
	}

	public void setSid(int sid) {
		this.sid = sid;
	}

	@Basic
	@Column(name = "name")
	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	@Basic
	@Column(name = "birthday")
	public Date getBirthday() {
		return birthday;
	}

	public void setBirthday(Date birthday) {
		this.birthday = birthday;
	}

	@Basic
	@Column(name = "address")
	public String getAddress() {
		return address;
	}

	public void setAddress(String address) {
		this.address = address;
	}

	@Override
	public boolean equals(Object o) {
		if (this == o) return true;
		if (o == null || getClass() != o.getClass()) return false;

		StudentsEntity that = (StudentsEntity) o;

		if (id != that.id) return false;
		if (sid != that.sid) return false;
		if (name != null ? !name.equals(that.name) : that.name != null) return false;
		if (birthday != null ? !birthday.equals(that.birthday) : that.birthday != null) return false;
		if (address != null ? !address.equals(that.address) : that.address != null) return false;

		return true;
	}

	@Override
	public int hashCode() {
		int result = id;
		result = 31 * result + sid;
		result = 31 * result + (name != null ? name.hashCode() : 0);
		result = 31 * result + (birthday != null ? birthday.hashCode() : 0);
		result = 31 * result + (address != null ? address.hashCode() : 0);
		return result;
	}
}
```

对应的映射表`StudentsEntity.hbm.xml`

```xml
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-mapping PUBLIC
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping>
  <class name="com.entity.StudentsEntity" table="Students" schema="Hibernate">
    <id name="id" column="id" />
    <property name="sid" column="sid" />
    <property name="name" column="name" />
    <property name="birthday" column="birthday" />
    <property name="address" column="address" />
  </class>
</hibernate-mapping>
```

# 通过 Hibernate Api 编写访问数据库的代码

## 首先创建配置对象

```java
Configuration config = new Configuration().configure();
```

## 创建服务注册对象

```java
ServiceRegistry serviceRegistry = new ServiceRegistryBuilder()
  .applySettings(config.getProperties())
  .buildServiceRegistry();
```

## 创建会话工厂对象

```java
sessionFactory = config.buildSessionFactory(serviceRegistry);
```

## 打开会话

```java
session = sessionFactory.openSession();
```

## 打开事务

```java
transaction = session.beginTransaction();
```

## 示例：存储

```java
mSession.save(new Student());

mTransaction.commit();
mSession.close();
mSessionFactory.close();
```

# hibernate.cfg.xml 常用配置

| 属性名字                     | 含义                                       |
| ------------------------ | ---------------------------------------- |
| Hibernate.show_sql       | 是否把 Hibernate运行时候的 SQL语句输出到控制台           |
| Hibernate.format_sql     | 输出到控制台的SQL 语句是否进行排版                      |
| hbm2ddl.auto             | 可以帮助由java 代码生成的数据库脚本，进而生成具体的表结构。create\|update\|create-drop\|validate |
| hibernate.default_schema | 默认的数据库                                   |
| hibernate.dialect        | 配置 Hibernate数据库的方言，Hibernate可针对特殊的数据库进行优化 |

> * create 每次删除原先的表，重新创建
> * update 保留数据，添加新数据
> * create-drop 先创建表，后删除
> * validate 对原来的表结构进行验证，如果现在和原来不同，则不创建

# Session

Session 的两种开启方法

## openSession



## getCurrentSession

如果使用`getCurrentSession`需要在`hibernate.cfg.xml`中配置

* 本地事务（jdbc 事务）

  * ```xml
    <propertyname="hibernate.current_session_context_class">thread</propertyname>
    ```

* 全局事务（jta 事务）

  * ```xml
    <propertyname="hibernate.current_session_context_class">jta</propertyname>
    ```

* 两者的区别

  * openSession 需要调用者自己管理 Session 的生命周期
  * getCurrentSession 则是将 Session 的生命周期封装起来，不需要调用者过度操心

# hbm 配置文件常用设置

```xml
<hibernate-mapping
  defaultschema="schemaName"	//模式名字
  catalog="catalogName"		//目录名称
  default-cascade="cascade_style"	//级联风格
  default-access="field|property|ClassName"		//访问策略
  default-lazy="true|false"		//加载策略
  package="packagename"
/>
```

## class 标签

```xml
<class
  name="ClassName"
  table="tableName"
  batch-size="N"		//抓取策略，一次抓取多少
  where="condition"		//抓取附加条件
  entity-name="EntityName"		//同一个实体类映射多张表时使用
/>
```

## id 标签（主键）

```xml
<id
  name="propertyName"
  type="typeName"
  column="column_name"
  length="length">
  <!-- 主键生成策略 -->
  <generator class="generatorClass" />
</id>
```

