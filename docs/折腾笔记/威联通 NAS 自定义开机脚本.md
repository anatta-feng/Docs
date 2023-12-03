Shell 下执行

```bash
mount $(/sbin/hal_app --get_boot_pd port_id=0)6 /tmp/config
```

即可挂载

然后

```bash
vim /tmp/config/autorun.sh
```

在里面写自己开机想执行什么

写完后

```bash
chmod +x /tmp/config/autorun.sh
```

然后取消挂载

```bash
umount /tmp/config
```

即可