Ubuntu 挂载 smd 需要先安装 smb

# 安装指令

```shell
sudo apt-get install samba
sudo apt-get install smbclient
sudo apt-get install cifs-utils
```

# 配置 smb

`sudo gedit /etc/samba/smb.conf`

在global下的workgroup=WORKGROUP下面添加一句：

```
security = user
```

然后在文件末尾添加：

```
[workspace]
   comment = sharefolder
   path = /home/mario/workspace
   valid users = mario
   browseable = yes
   read only = yes
   create mask = 0777
   directory mask = 0777
   public = yes
   writable = yes
   available = yes
```

注意：

path：/home/mario/workspace表示你想共享的文件夹名称，首先你要在ubuntu下创建这个文件夹workspace，并执行chmod添加权限：

sudo chmod 777 /home/mario/workspace
valid users：表示你允许访问该共享的用户

保存后重新启动samba

```
sudo /etc/init.d/samba restart
或者 sudo /etc/init.d/smbd restart
可以cd到 /etc/init.d/下查看是哪个（samba或者smbd）
```

3.设置用户和密码
```
sudo smbpasswd -a mario
输入两次密码后就可以了。
```

smbpasswd的用法：

```
smbpasswd -a users：增加用户users

smbpasswd -d users：冻结用户users，这个用户不能再登录

smbpasswd -e users：恢复冻结的用户users，让冻结的用户可以再使用

smbpasswd -x users：删除用户users
``

# 挂载指令
1. 列举指定IP地址所提供的共享文件夹列表
	1. `smbclient -L ${ip_addr} -U ${username}%${password}`
2. 挂载共享文件夹
	1. `mount -t cifs ${remount_share_folder}  ${local_mount_folder} -o username=${username},password=${password}
	2. `eg. mount -t cifs //192.168.1.1/share /mnt/share -o username=root,password=123456`

# 遇到的问题

1. mount error(95): Operation not supported
	1. 需要指定版本：``mount -t cifs ${remount_share_folder}  ${local_mount_folder} -o username=${username},password=${password},vers=3.0`
2. 