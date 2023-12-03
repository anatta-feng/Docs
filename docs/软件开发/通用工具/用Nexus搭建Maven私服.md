> Maven仓库可以代理远程仓库和部署自己或第三方构件

# 环境

Java

# 安装Nexus

## 下载

[官网链接](https://www.sonatype.com/download-oss-sonatype)

## 解压

```shell
tar -xvf nexus-[version].tar.gz
```

## 启动

```shell
cd nuxus-[version]/bin
./nexus start
```

## 开启远程访问端口

关闭防火墙，并开启远程访问端口8081

```shell
vim /etc/sysconfig/iptables
添加：
-A INPUT -p tcp -m state --state NEW -m tcp --dport 8081 -j ACCEPT
```

## 测试

nexus默认端口是8081

默认账号：admin

默认密码：admin123

# 其他

## 修改nexus默认端口

```shell
cd /usr/local/nexus-3.6.0-02/etc/
vim nexus-default.properties
```

**默认端口: 8081**

```
application-port=8081
```

##修改 nexus 数据以及相关日志的存储位置

```
[root@MiWiFi-R3-srv bin]# cd /usr/local/nexus-3.6.0-02/bin/
[root@MiWiFi-R3-srv bin]# vim nexus.vmoptions
-XX:LogFile=./sonatype-work/nexus3/log/jvm.log
-Dkaraf.data=./sonatype-work/nexus3
-Djava.io.tmpdir=./sonatype-work/nexus3/tmp
```
