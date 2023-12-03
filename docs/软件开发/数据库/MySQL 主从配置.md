MySQL 数据库提供了一种主从备份的机制，其实就是把主数据库的所有的数据同时写到备份的数据库中。实现mysql数据库的热备份。

# 主服务器配置

1. 创建 slave 账户，用于同步数据

```sql
CREATE USER 'slave'@'%' IDENTIFIED BY 'Password';
GRANT REPLICATION SLAVE ON *.* TO 'slave'@'%';
```
2. 修改数据库配置 `my.cnf`，然后重启数据库
```conf
# 开启 bin log
log-bin = mysql-bin  
# 设置 server id，确保主从的 server id 不一致
server-id = 1
```
3. 锁库，同步状态
```sql
FLUSH TABLES WITH READ LOCK;
```
4. 获取主库的二进制日志名和偏移量，这两个变量是同步建立的关键
```sql
SHOW MASTER STATUS;

+------------------+----------+--------------+------------------+  
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |  
+------------------+----------+--------------+------------------+  
| mysql-bin.000016 | 537      |              |                  |  
+------------------+----------+--------------+------------------+
```
5. 备份主库的数据
6. 解锁主库
```sql
UNLOCK TABLES;
```
# 从库配置
1. 编辑从库的配置文件 `my.cnf`，然后重启数据库
```conf
server-id = 2
```
2. 将主库的备份数据恢复到从库中
3. 开启从库的 slave 配置
```sql
STOP SLAVE;
RESET MASTER;
CHANGE MASTER TO MASTER_HOST='timezapp.top', MASTER_PORT=60336, MASTER_USER='slave', MASTER_PASSWORD='pwssword', MASTER_LOG_FILE='mysql-bin.000003', MASTER_LOG_POS=154;
START SLAVE; 
```
4. 查看 Slave 状态
```sql
SHOW SLAVE STATUS;
```
需要确保 `Slave_SQL_Running` 和 `Slave_IO_Running` 都为 `YES`，否则需要根据报错来修复问题。

# 小插曲

本来我是在家里的 NAS 上配置从库的，但是 Docker 中的 Mysql 居然无法修改挂载到物理机的配置文件（配置文件修改后不起作用）。至今不知道原因，等后续知道原因了再修复。