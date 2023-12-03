
>`{}`代表必选
>
>`|`代表几个选项中选择其中一个
>
>`[]`代表可选

# 连接数据库

```shell
mysql -uroot -p [-P 端口号] [-h服务器 ip 地址]
```



# 一般命令

* SELECT USER();
  * 查看当前用户
* SELECT VERSION();
  * 查看 mysql 版本
* SHOW WARNINGS
  * 查看警告信息
* SELECT DATABASE();
  * 查看用户当前打开的数据库
* SHOW INDEXES FROM tbl_name;
  * 显示索引

## 查看数据库、表等创建指令

### 数据库

```mysql
SHOW CREATE DATABASE db_name;
```

## 表

```mysql
SHOW CREATE TABLE db_name.tb_name;
|
USE DATABASE db_name;
SHOW CREATE TABLE tb_name;
```




# 数据库操作

## 创建数据库

```mysql
CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [DEFAULT] CHARACTER SET [=] charset_name
```

其中`CHARACTER SET [=] charset_name`是设置数据库编码字符集

---

## 查看当前服务器下的数据表列表

```mysql
SHOW {DATABASES | SHEMAS} [LIKE 'pattern' | WHERE expr]
```

---

## 数据库的修改

### 数据库

```mysql
ALTER {DATABASE | SCHEMA} [db_name] [DEFAULT] CHARACTER SET [=] charset_name;
```

---

### 表

```mysql
ALTER {TABLE} [tb_name] [DEFAULT] CHARACTER SET [=] charset_name;
```

---

##删除数据库

```mysql
DROP {DATABSE | SCHEMA} [IF EXISTS] da_name;
```

---

# 数据库的数据类型

> 数据类型是指列、存储过程参数、表达式和局部变量的数据特征，它决定了数据的存储格式，代表了不同的信息类型

## 整型

|   数据类型    | 存储范围                                     |  字节  |
| :-------: | ---------------------------------------- | :--: |
|  TINYINT  | 有符号值：-128到127 无符号值：0到255                 |  1   |
| SMALLINT  | 有符号值：-32768到32767  无符号值：0到65535          |  2   |
| MEDIUMINT | 有符号值：-8388608到8388607 无符号值：0到16777215    |  3   |
|    INT    | 有符号值：-2147483648到2147483647 无符号值：0到429467295 |  4   |
|  BIGINT   | 有符号值：-9223372036854775808到9223372036854775809 无符号位：8到1844676744073709551615 |  8   |

## 浮点型

|     数据类型      | 说明                                       |
| :-----------: | :--------------------------------------- |
| FLOAT[(M,D)]  | M 是数字总位数，D 是小数点后面的位数，如果 M 和 D 被省略，根据硬件允许的限制来保存值。单精度浮点数精确到大约七位小数 |
| DOUBLE[(M,D)] | 两者除了存储范围不一样，小数点后面的位数也不一样                 |

---

## 日期时间型

| 列类型       | 存储需求 |
| --------- | ---- |
| YEAR      | 1    |
| TIME      | 3    |
| DATE      | 3    |
| DATETIME  | 8    |
| TIMESTAMP | 4    |

---

## 字符型

| 列类型                          | 存储需求                                |
| ---------------------------- | ----------------------------------- |
| CHAR(M)                      | M个字节，0<=M<=255                      |
| VARCHAR(M)                   | L+1个字节，其中 L<=M 且0<=M<=65535         |
| TINYTEXT                     | L+1个字节，其中 L<2^8                     |
| TEXT                         | L+2个字节，其中 L<2^16                    |
| MEDIUMTEXT                   | L+3个字节，其中 L<2^24                    |
| LONGTEXT                     | L+4个字节，其中 L<2^32                    |
| ENUM('value', 'value2', ...) | 1或2个字节，取决于枚举值的个数（最多65535个值）         |
| SET('value1', 'value2', ...) | 1，2，3，4或8个字节，取决于 set 成员的数目（最多64个成员） |

---

## 打开数据库

```mysql
USE db_name;
```

# 数据表

## 创建数据表

```mysql
CREATE TABLE [IF NOT EXISTS] table_bane (colum_name data_type [约束], ...)
```

---

## 查看数据表

```mysql
SHOW TABLES [FROM db_name] [LIKE 'pattern' | WHERE expr]
```

---

## 查看数据表结构

```mysql
SHOW COLUMNS FROM tbl_name;
```

---

## 记录的插入与查询

### INSERT

```mysql
INSERT [INTO] tbl_name [(col_name,...)] VALUES(val,...);
```

---

### SELECT

> 简易版

```mysql
SELECT expr,... FROM tbl_name;
```

---

## 修改数据表

### 添加单列

```mysql
ALTER TBLE tbl_name ADD [COLUMN] col_name column_definition [FIRST | AFTER col_name]
```

---

### 添加多列

```mysql
ALTER TABLE tbl_name ADD [COLUMN] (col_name column_definition,...);
```

---

### 删除列

```mysql
ALTER TABLE tbl_name DROP [COLUMN] col_name;
```

---

### 复合操作

> 同时删除或添加多个列

```mysql
ALTER TABLE tbl_name DROP [COLUMN] col_name, DROP col_name,...ADD col_name column_definition, ...;
```

---

## 添加约束

### 添加/删除主键约束

#### 添加

```mysql
ALTER TABLE tbl_name ADD [CONSTRAINT [symbol]] PRIMARY KEY [index_type] (index_col_name,...)
```

#### 删除

```mysql
ALTER TABLE tbl_name DROP PRIMARY KEY
```

---

### 添加/删除唯一约束

#### 添加

```mysql
ALTER TABLE tbl_name ADD [CONSTRAINT [symbol]] UNIQUE [INDEX|KEY] [index_name] [index_type] (index_col_name,...)
```

#### 删除

```mysql
ALTER TABLE tbl_name DROP {INDEX|KEY} index_name
```

---

### 添加/删除外键约束

#### 添加

```mysql
ALTER TABLE tbl_name ADD [CONSTRAINT [symbol]] FOREIGN KEY [index_name] (index_col_name,...) reference_definition
```

#### 删除

```mysql
ALTER TABLE tbl_name DROP FOREIGN KEY flk_symbol
```

---

### 添加/删除默认约束

```mysql
ALTER TABLE tbl_name ALTER [COLUMN] col_name {SET DEFAULT literal | DROP DEFAULT}
```

## 修改列定义

```mysql
ALTER TABLE tbl_name MODIFY [COLUMN] col_name column_definition [FIRST | AFTER col_name]
```

## 修改列名称

```mysql
ALTER TABLE tbl_name CHANGE [COLUMN] old_col_name new_col_name column_defintion [FIRST|AFTER col_name]
```

## 数据表更名

### 方法1

```mysql
ALTER TABLE tbl_name RENAME [TO|AS] new_tbl_name
```

### 方法2

```mysql
RENAME TABLE tbl_name TO new_tblname [,tbl_name2 TO new_tbl_name2,...]
```







# MySQL 属性

## 无负数

> UNSIGNED

---

## 空值与非空

NULL，字段可以为空

NOT NULL, 字段值禁止为空

---

## 自动编号

> AUTO_INCREMENT

* 自动编号，必须与主键组合使用
* 默认情况下，起始值为1，每次增量为1

---

## 主键

> PRIMARY KEY

* 主键约束
* 每张数据表只能存在一个主键
* 主键保证记录的唯一性
* 主键自动为 NOT NULL

---

## 唯一约束

> UNIQUE KEY

* 唯一约束
* 唯一约束可以保证记录的唯一性
* 唯一约束的字段可以为空值(NULL)
* 每张数据表可以存在多个唯一约束

---

## 默认约束

> DEFAULT

* 默认值
* 当插入记录时，如果没有明确为字段复制，则自动赋予默认值

---





# 约束

1. 约束保证数据的完整性和一致性
2. 约束分为表级约束和列级约束
3. 约束类型包括：
   1. NOT NULL(非空约束)
   2. PRIMARY KEY（主键约束）
   3. UNIQUE KEY（唯一约束）
   4. DEFAULT（默认约束）
   5. FOREIGN KEY（外键约束）

---

## 表级约束和列级约束

> 对一个数据列建立的约束，成为列级约束
>
> 对多个数据列建立的约束，成为表级约束。
>
> 列级约束既可以在列定义时声明，也可以在列定义后声明
>
> 表级约束只能在列定义后声明

`NOT NULL` 与 `DEFAULT`约束不存在表级约束

e.g

```mysql
CREATE TABLE test (
  id SMALLINT UNSIGNED NOT NULL AUTO_INCREMENT,
  name VARCHAT NOT NULL,
  PRIMARY KEY (id)
)
```



## 外键

> 作用：
>
> * 保证数据的一致性，完整性
> * 实现一对一或一对多关系

## 使用

```mysql
CREATE TABLE test (
  id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT.
  pid SMALLINT UNSIGNED,
  FOREIGN KEY (pid) REFERENCES father_tbl_name (f_cl_name) [参照操作]
)；
```



### 外键约束的要求

> 有外键的表为字表，被参照的是父表

1. 父表和子表必须使用相同的存储引擎，而且禁止使用临时表
2. 数据表的存储引擎只能为 `InnoDB`
3. 外键列和参照列必须具有相似的数据类型。其中数字的长度或是否有符号位bi'xu'xl'ts必须相同；而字符的长度可以不同
4. 外键列和参照列必须创建索引。如果外键列不存在索引的话，MySQL 将自动创建索引

---

### 外键约束的参照操作

1. CASCADE: 从父表删除或更新切自动删除或更新子表中匹配的行
2. SET NULL: 从父表删除或更新行，并设置子表中的外键为 NULL。如果使用该选项，必须保证子表列没有指定 NOT NULL
3. RESTRICT： 拒绝对父表的删除或更新操作
4. NO ACTION: 标准 SQL 的关键字，在 MySQL 中与 RESTRICT 相同

---

# 数据库值的操作

## INSERT

插入记录

```mysql
INSER [INTO] tbl_name [(col_name,...)] {VALUES|VALUE} ({exper|DEFAULT},...),(...),...
```

## UPDATE

### 更新记录（单表更新）

```mysql
UPDATE [LOW_PRIORITY] [IGNORE] table_reference SET col_name1={exper1|DEFAULT} [,col_name2={expr2|DEFAULT}]...[WHERE where_cpndition]
```

## DELETE

### 删除记录（单表删除）

```Mysql
DELETE FROM tbl_name [WHERE where_condition]
```

## 查询表达式解析

> SELECT

查找记录

```mysql
SELECT select_expr [,select_expr,...] 
[
  FROM table_references
  [WHERE where_condition]
  [GROUP BY {col_name|position} [ASC|DESC],...]
  [HAVING where_condition]
  [ORDER BY {col_name|expr|position} [ASC|DESC],...]
  [LIMIT {[offset,] row_count|row_count OFFSET offset}]
]
```

---

### select_expr

查询表达式

> 每一个表达式表示想要的一列，必须有至少一个。
>
> 多个列之间以英文逗号分隔。
>
> 星号(*)表示所有列。tbl_name.\* 可以表示命名表的所有列。
>
> 查询表达式可以使用[AS]alias_name 为其赋予别名。
>
> 别名可用于 GROUP BY, ORDER BY 或 HAVING 字句。


# 创建用户并授权

```sql
CREATE USER '$dog'@'localhost' IDENTIFIED BY '$passwd$';
GRANT $privileges ON $databasename.$tablename TO '$username'@'host'
```

## 说明:

-   privileges：用户的操作权限，如`SELECT`，`INSERT`，`UPDATE`等，如果要授予所的权限则使用`ALL`
-   databasename：数据库名
-   tablename：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用`*`表示，如`*.*`


## mysql "select command denied to user root" 问题解决

找到 mysql 数据库下的 user 表，把对应的用户权限修改为 'Y'


![[nav_sql.jpg]]