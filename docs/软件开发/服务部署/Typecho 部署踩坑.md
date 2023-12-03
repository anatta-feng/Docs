#typecho 

> 在下近期用 Typecho 搭建了博客平台，但是其中真的太过艰辛，尤其作为一个对 PHP 丁点儿不懂的菜🐔开发，更是难受，耗时将近两天。而且网上资料太少，在此稍微记录下过程中遇到的坑，希望对后人能有些许帮助。

# 用 Docker 部署

如果你想用 Docker 部署，却对 Docker 不太懂，推荐你从 [Docker Practice](https://github.com/yeasy/docker_practice) 入门，并且我也有一篇 [Docker 新手踩坑](https://blog.fxcdev.com/archives/11/)，希望能够有点帮助。

而且我这里有一份 [`docker-compose.yml`](https://gist.github.com/emrysf/39b4220e0247a38f03d99a54bca15c00) 可以供你参考。

# 部署好了以后访问 File not found

**这个解决方案的前提是使用 nginx 转发 fpm 伪静态部署方案，而不是 nginx 代理 Apache 。**~~既然 nginx 也能当服务器为什么要多开一个 Apache 呢？~~

首先要确定你的路径真的正确，怎么确定呢？其实去看 nginx 的 `error.log` 就可以初步确认了。

如果 `error.log` 里有这么一行：

```log
FastCGI sent in stderr: "Primary script unknown" while reading response header from upstream,....
```

那么基本可以确定，就是你的路径有问题，或者环境变量没有设置好，导致 fpm 找不到 php 文件。

解决方案添加或修改如下配置：

```nginx
fastcgi_param  SCRIPT_FILENAME  /[your index.php path]/$fastcgi_script_name;
```

比如：

```nginx
location ~ .*\.php(\/.*)*$ {
  include fastcgi.conf;
  include fastcgi_params;
  fastcgi_param  SCRIPT_FILENAME  /www/$fastcgi_script_name;

  fastcgi_pass   blog:9000;
}
```

相信配置过 nginx 的一看就懂吧。

# 啥都弄好了，就是安装的时候数据库只有 SQLite，没有 MySQL

这个可能是 PHP 环境的问题，我开始一直用的官方的 PHP 镜像，就是这个原因，其实也安装了 MySQL 扩展，但是就是不行，整的人很懵逼。后面换镜像了，换成 `hteen/php` 后，并且配置上 `php.ini` 配置文件后，终于看到 MySQL 了。

我对 PHP 也不懂，不能给什么建设性建议，只能附上我的 [php.ini](https://gist.github.com/emrysf/a31c4c599dbb2fe51e70c8b7e304e287) 希望能帮助你。

对了，配置文件的映射路径上面的 `docker-compose.yml` 里有。

# 安装的时候显示无法连接数据库

这个问题其实千奇百怪的原因都有，这里吐槽一下 Typecho 的报错机制，简直就是 Java 中的 `catch(Exception e) {return "error"}`，除了知道出错了其他啥信息都不知道。

这个问题的排查思路：

## 检查数据库配置是否正确

- [ ] 数据库地址是否正确
- [ ] 数据库端口是否正确（这两项如果是 Docker 那么就是容器别名和容器内部端口，当然你也可以上公网 IP 和公网端口）
- [ ] 数据库用户名和密码是否正确

如果这些都没问题，进入下一步

## 数据库中是否已经建立了 typecho 数据库

因为 Typecho 不会自动键库的，需要你自己手动建库，名字在安装页可以配置。当然我并没有配置，用的默认的。~~因为我也不知道配置后有没有 Bug，懒。~~

如果你数据已经建了，还是报无法连接数据库，那么往下走。

## PHP 低版本无法和 mysql 8.0 连接

如果你的 mysql 是 8.0 版本的（含以上），那么有可能是这个原因，因为 mysql 8.0 默认是使用caching_sha2_password 加密插件的， PHP 有的版本是不兼容这个加密插件的。

怎么整呢？两个选择：

1. 更新 PHP 版本到兼容的版本
2. mysql 改成旧版本的加密插件。

虽然方法 2 会降低安全性，但是我还是选择了方法 2。~~真的快让 Typecho 逼疯了。~~当然，如果你也选择了方法 2，那么建议给 Typecho 单独开一个数据库账号，并且限定权限，我对于每个项目都是这么干的。然后还要定期备份数据（就算不降级也要这么干）。

怎么降级加密插件呢？进入 mysql，如果新增用户则执行：

```mysql
CREATE USER 'username'@'localhost' identified with mysql_native_password by 'password'
```

如果改变已有用户则执行：

```mysql
ALTER USER 'username'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

你问我一个对 PHP 什么都不懂的人怎么发现这个问题的？当然是改源码了。

### 修改源码打印具体错误

修改 `install.php`，通过搜索 `对不起，无法连接数据库，请先检查数据库配置再继续进行安装` 可以找到如下代码：

```php
try {
    $installDb->query('SELECT 1=1');
} catch (Typecho_Db_Adapter_Exception $e) {
    $success = false;
    echo '<p class="message error">'
    . $e
    . _t('对不起，无法连接数据库，请先检查数据库配置再继续进行安装') . '</p>';
} catch (Typecho_Db_Exception $e) {
    $success = false;
    echo '<p class="message error">'
    . _t('安装程序捕捉到以下错误: " %s ". 程序被终止, 请检查您的配置信息.',$e->getMessage()) . '</p>';
}
```

看到第 6 行了嘛？那个 `. $e` 就是我添加的打印，会把错误直接打印在网页上，然后根据具体的异常去搜索解决方案吧，事半功倍！

# 结语

总算折腾够了，真的累人，不过**生命不止，折腾不息**嘛～希望可以帮助到后面的人，别再无头苍蝇到处乱撞，净做些无用功了。

