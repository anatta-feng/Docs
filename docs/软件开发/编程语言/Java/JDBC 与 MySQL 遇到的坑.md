---
title: JDBC 与 MySQL 遇到的坑
date: 2017-6-15 10:05:25
categories: [笔记]
tags: [数据库]
---

# 远程连接数据库

需要配置：

## 允许 ROOT 用户在任何地方登录，并具有任何权限

首先，登录 mysql

在本机先使用root用户登录mysql：
`mysql -u root -p"youpassword"`

进行授权操作：
`mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'youpassword' WITH GRANT OPTION;`

重载授权表：
`FLUSH PRIVILEGES;`

退出mysql数据库：
`exit`

## 允许 ROOT 用户在一个特定的IP 进行远程登录，并具有任何权限

在本机先使用root用户登录mysql：
`mysql -u root -p"youpassword" `
进行授权操作：
`GRANT ALL PRIVILEGES ON *.* TO root@"172.16.16.152" IDENTIFIED BY "youpassword" WITH GRANT OPTION;`
重载授权表：
`FLUSH PRIVILEGES;`
退出mysql数据库：
`exit`

---

## 允许 ROOT 用户在特定 ip 登录，具有特定权限

在本机先使用root用户登录mysql：
`mysql -u root -p"youpassword" `
进行授权操作：
`GRANT select，insert，update，delete ON *.* TO root@"172.16.16.152" IDENTIFIED BY "youpassword";`
重载授权表：
`FLUSH PRIVILEGES;`
退出mysql数据库：
`exit`

---

## 删除用户授权

`REVOKE privileges ON 数据库[.表名] FROM user-name;`
具体实例，先在本机登录mysql:
`mysql -u root -p"youpassword" `
进行授权操作：
`GRANT select，insert，update，delete ON TEST-DB TO test-user@"172.16.16.152" IDENTIFIED BY "youpassword";`
再进行删除授权操作：
`REVOKE all on TEST-DB from test-user;`
**注：该操作只是清除了用户对于TEST-DB的相关授权权限，但是这个“test-user”这个用户还是存在。**
最后从用户表内清除用户：
`DELETE FROM user WHERE user="test-user";`
重载授权表：
`FLUSH PRIVILEGES;`
退出mysql数据库：
`exit`

---

## MYSQL权限详细分类

全局管理权限： 
FILE: 在MySQL服务器上读写文件。 
PROCESS: 显示或杀死属于其它用户的服务线程。 
RELOAD: 重载访问控制表，刷新日志等。 
SHUTDOWN: 关闭MySQL服务。
数据库/数据表/数据列权限： 
ALTER: 修改已存在的数据表(例如增加/删除列)和索引。 
CREATE: 建立新的数据库或数据表。 
DELETE: 删除表的记录。 
DROP: 删除数据表或数据库。 
INDEX: 建立或删除索引。 
INSERT: 增加表的记录。 
SELECT: 显示/搜索表的记录。 
UPDATE: 修改表中已存在的记录。
特别的权限： 
ALL: 允许做任何事(和root一样)。 
USAGE: 只允许登录--其它什么也不允许做。

---

# JDBC 无法连接数据库

Access denied for user

代码正确的情况下有两种情况

1. 你的用户名或者密码确实错误，这个情况最简单，只要把用户名和密码修改正确就可以了。
2. 用户名和密码都正确，此时你可以使用如下命令通过终端进行登录测试：

`mysql -u root -p  (这里使用root帐号登录数据库)`

```mysql
GRANT ALL ON bookdb.*  TO 'user'@'localhost' IDENTIFIED BY 'passwd' WITH GRANT OPTION;（给用户赋权）
```

```mysql
FLUSH PRIVILEGES;（使用赋权生效）
```

如果这样赋权之后，使用JDBC还是不能连接，那么请再次使用如下命令：

```mysql
GRANT ALL ON bookdb.*  TO 'user'@'127.0.0.1' IDENTIFIED BY 'passwd' WITH GRANT OPTION;（给用户赋权）
```

```mysql
FLUSH PRIVILEGES;（使用赋权生效）
```

