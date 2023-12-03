> 既然要用 docker，那就所有的服务都用 docker。本篇文章记录下用 docker 部署 mysql

docker 仓库里有现成的 mysql 镜像可以使用，

最简单的命令`docker run -d mysql/mysql-server` 可以直接跑起来一个 mysql 实例，但是这远远达不到我们的要求。

# 要求

1. 数据持久化，需要挂载路径
2. 换端口，防止默认端口被爆破

# 命令

`docker run --name $(name) -p $(port):3306 -v $(path):/var/lib/mysql -e MYSQL_ROOT_PASSWORD=$(passwd) -d mysql`

# MySQL 配置

数据库建立好了，但是只有 root 用户，需要为项目配置用户。

通过这行命令`docker exec -it mysql_name /bin/bash`进入镜像。

然后进入 mysql

## 新增用户

```mysql
CREATE USER vaipixel IDENTIFIED BY 'passwd';
```

## 为用户配置权限

```mysql
GRANT ALL PRIVILEGES ON vaipixel.* to vaipixel@'%';
```

这行命令是赋予了 vaipixel 用户在 vaipixel 数据库上所有的权限，并且允许远程登录。

```mysql
FLUSH PRIVILEGES;
```

刷新一下

# 结语

现在 mysql 环境已经准备好了，可以直接用了，而且镜像关闭后数据不会丢，重新开启一个镜像，只要数据挂载路径一样，就可以直接用之前的数据

