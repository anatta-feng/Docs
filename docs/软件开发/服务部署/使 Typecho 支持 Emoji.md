如果是 Typecho 安装之前，那好办，建库和安装的时候都选择编码为 `utf8mb4` 即可（Typecho 开发版需要选择数据库编码格式）。

如果是 Typecho 安装后改动，那会稍微麻烦点，但是也不是很麻烦：

# 1. 修改数据库编码

将数据库的编码改为 `utf8mb4`

## 2. 修改数据表编码

直接在数据库中运行以下 SQL

```mysql
alter table typecho_comments convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_contents convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_fields convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_metas convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_options convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_relationships convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_users convert to character set utf8mb4 collate utf8mb4_unicode_ci;
```

# 3. 修改 Typecho 数据库配置文件

网站根目录数据库配置文件 `config.inc.php`

```php
$db->addServer(array (
  'host'      =>  localhost,
  'user'      =>  'youruser',
  'password'  =>  'yourpassword',
  'charset'   =>  'utf8mb4', //修改这一行
  'port'      =>  3306,
  'database'  =>  'yourdatabase'
), Typecho_Db::READ | Typecho_Db::WRITE);
```

# 大功告成