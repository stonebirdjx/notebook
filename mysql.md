<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [mysql常用命令](#mysql%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)
- [常用](#%E5%B8%B8%E7%94%A8)
- [mysql 工具简介](#mysql-%E5%B7%A5%E5%85%B7%E7%AE%80%E4%BB%8B)
- [数据库管理](#%E6%95%B0%E6%8D%AE%E5%BA%93%E7%AE%A1%E7%90%86)
  - [启动](#%E5%90%AF%E5%8A%A8)
  - [系统变量](#%E7%B3%BB%E7%BB%9F%E5%8F%98%E9%87%8F)
  - [状态变量](#%E7%8A%B6%E6%80%81%E5%8F%98%E9%87%8F)
  - [:point_right:关机过程](#point_right%E5%85%B3%E6%9C%BA%E8%BF%87%E7%A8%8B)
  - [:point_right:权限](#point_right%E6%9D%83%E9%99%90)
  - [:point_right:用户管理](#point_right%E7%94%A8%E6%88%B7%E7%AE%A1%E7%90%86)
  - [:point_right:备份与恢复](#point_right%E5%A4%87%E4%BB%BD%E4%B8%8E%E6%81%A2%E5%A4%8D)
  - [表维护和崩溃恢复](#%E8%A1%A8%E7%BB%B4%E6%8A%A4%E5%92%8C%E5%B4%A9%E6%BA%83%E6%81%A2%E5%A4%8D)
  - [:point_right:MySQL日志文件](#point_rightmysql%E6%97%A5%E5%BF%97%E6%96%87%E4%BB%B6)
- [:point_right:SQL优化](#point_rightsql%E4%BC%98%E5%8C%96)
  - [:point_right:优化select语句](#point_right%E4%BC%98%E5%8C%96select%E8%AF%AD%E5%8F%A5)
    - [WHERE子句优化](#where%E5%AD%90%E5%8F%A5%E4%BC%98%E5%8C%96)
  - [优化索引](#%E4%BC%98%E5%8C%96%E7%B4%A2%E5%BC%95)
  - [优化数据结构](#%E4%BC%98%E5%8C%96%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
  - [优化InnoDB表](#%E4%BC%98%E5%8C%96innodb%E8%A1%A8)
- [:point_right:InnoDB存储引擎](#point_rightinnodb%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E)
- [:point_right:MyISAM存储引擎(一般弃用)](#point_rightmyisam%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E%E4%B8%80%E8%88%AC%E5%BC%83%E7%94%A8)
- [:point_right: 主从](#point_right-%E4%B8%BB%E4%BB%8E)
- [建议](#%E5%BB%BA%E8%AE%AE)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# mysql常用命令

` mysql [OPTIONS] [database]`

```bash
--auto-rehash       # 自动补全功能，
-D, --database=name # 使用数据库  
-e, --execute=name  # 执行mysql的sql语句
-E, --vertical      # 垂直打印查询输出  
-f, --force         # 如果有错误跳过去，继续执行下面的  
-p, --password[=name] # 输入密码  
-h, --host=name     # 设置连接的服务器名或者Ip  
-P, --port=port       # 设置端口  
--prompt=name       # 设置mysql提示符
-u, --user=name     # 用户名

# 例子
mysql -u root -p ${your_password} stonebird
```

常用指令

```bash
?         (\?) Synonym for `help'.
exit      (\q) Exit mysql. Same as quit.
quit      (\q) Quit mysql.
source    (\.) Execute an SQL script file. Takes a file name as an argument.
status    (\s) Get status information from the server.、
show           show databases or tables
use       (\u) Use another database. Takes database name as argument.
charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
```

# 常用

```bash
# 查当前用户、日期、数据库
SELECT USER(), CURRENT_DATE;
SELECT DATABASE();

# 查询密码密文
SELECT password('test1');

# 创建表
CREATE TABLE pet (
    name VARCHAR(20), 
    owner VARCHAR(20),
    species VARCHAR(20), 
    sex CHAR(1), 
    birth DATE,
    death DATE
);

# 查询表 
SHOW TABLES;

# 要将文本文件加载到表
LOAD DATA LOCAL INFILE '/path/pet.txt' INTO TABLE pet;

# 插入数据
INSERT INTO table_name ( field1, field2,...fieldN )
VALUES  ( value1, value2,...valueN );
# 例子
INSERT INTO pet
VALUES ('Puffball','Diane','hamster','f','1999-03-30',NULL);

# 修改数据
UPDATE pet SET birth = '1989-08-31' WHERE name = 'Bowser';

# 查询语句
SELECT * FROM pet 
WHERE (species = 'cat' AND sex = 'm')
OR (species = 'dog' AND sex = 'f');
# 特定行
SELECT name, birth FROM pet;

# 排序
SELECT name, birth 
FROM pet 
ORDER BY birth;
# 降序
SELECT name, birth 
FROM pet 
ORDER BY birth 
DESC;

# 日期计算 TIMESTAMPDIFF
SELECT name, birth, CURDATE(),
TIMESTAMPDIFF(YEAR,birth,CURDATE()) AS age
FROM pet;

# 匹配 以什么开头
SELECT * FROM pet WHERE name LIKE 'b%';
# 匹配 以什么结尾
SELECT * FROM pet WHERE name LIKE '%b';
# 匹配 包含
SELECT * FROM pet WHERE name LIKE '%wb%';
# 匹配单个字符 _ 
SELECT * FROM pet WHERE name LIKE 'h_x';
# REGEXP_LIKE 正则表达式。
SELECT * FROM pet WHERE REGEXP_LIKE(name, '^b');

# 行统计
SELECT COUNT(*) 
FROM pet;

# 分组
SELECT species, COUNT(*) 
FROM pet 
GROUP BY species;

# 多表查询
SELECT p1.name, p1.sex, p2.name, p2.sex, p1.species
FROM pet AS p1 
INNER JOIN pet AS p2
ON p1.species = p2.species
AND p1.sex = 'f' 
AND p1.death IS NULL
AND p2.sex = 'm' 
AND p2.death IS NULL;

# 显示表 
SHOW TABLES;

# 查看表信息 DESCRIBE table;
DESCRIBE pet;

# 命令行执行
mysql -e "source batch-file"
mysql < batch-file > mysql.out

# 导入sql
source filename.sql

# 最大值
SELECT MAX(article) AS article FROM shop;
# eg
SELECT article, dealer, price
FROM   shop
WHERE  price=(SELECT MAX(price) FROM shop);、

# 查询多个值
SELECT field1_index, field2_index
FROM test_table WHERE field1_index = '1'
UNION
SELECT field1_index, field2_index
FROM test_table WHERE field2_index = '1';

# 自增字段
CREATE TABLE animals (
     id MEDIUMINT NOT NULL AUTO_INCREMENT,
     name CHAR(30) NOT NULL,
     PRIMARY KEY (id)
);

INSERT INTO animals (name) VALUES
    ('dog'),('cat'),('penguin'),
    ('lax'),('whale'),('ostrich');

SELECT * FROM animals;
```

# mysql 工具简介

1、MYSQL服务器和服务器启动脚本：

- mysqld是MySQL服务器

- mysqld_safe、mysql.server和mysqld_multi是服务器启动脚本

- mysql_install_db初始化数据目录和初始数据库

2、 访问服务器的客户程序：

- **mysql**是一个命令行客户程序，用于交互式或以批处理模式执行SQL语句。
- **mysqladmin**是用于管理功能的客户程序。
- **mysqlcheck**执行表维护操作。
- **mysqldump**和**mysqlhotcopy**负责数据库备份。
- **mysqlimport**导入数据文件。 
- **mysqlshow**显示信息数据库和表的相关信息。

3、独立于服务器操作的工具程序：

- **myisamchk**执行表维护操作。
- **myisampack**产生压缩、只读的表。
- **mysqlbinlog**是处理二进制日志文件的实用工具。
- **perror**显示错误代码的含义。

# 数据库管理

## 启动

一般情况，用**mysql.server**脚本启动MySQL Database Server（MySQL数据库服务器），通常驻留在/etc/init.d/ 文件夹。默认情况下该脚本调用**mysqld_safe**脚本。

## 系统变量

你可以通过SHOW VARIABLES语句查看系统变量及其值。

```bash
SHOW VARIABLES;
SHOW VARIABLES LIKE 'auto_inc%';
```

## 状态变量

```bash
SHOW STATUS;
```

## :point_right:关机过程

服务器关闭进程可以概括为：

- 启动关闭进程
- 服务器根据需要创建关闭线程
- 服务器停止接收新连接
- 服务器终止当前的活动
- 存储引擎被停掉或关闭
- 服务器退出

## :point_right:权限

在tables_priv、columns_priv和procs_priv表中，权限列被声明为SET列。这些列的值可以包含该表控制的权限的组合：

| **表名**     | **列名**    | **可能的设置元素**                                           |
| ------------ | ----------- | ------------------------------------------------------------ |
| tables_priv  | Table_priv  | 'Select', 'Insert', 'Update', 'Delete', 'Create', 'Drop', 'Grant', 'References', 'Index', 'Alter' |
| tables_priv  | Column_priv | 'Select', 'Insert', 'Update', 'References'                   |
| columns_priv | Column_priv | 'Select', 'Insert', 'Update', 'References'                   |
| procs_priv   | Proc_priv   | 'Execute', 'Alter Routine', 'Grant'                          |

mysql提供的权限

GRANT和REVOKE语句所用的涉及权限的名称显示在下表，还有在授权表中每个权限的表列名称和每个权限有关的上下文。

| **权限**                | **列**                | **上下文**             |
| ----------------------- | --------------------- | ---------------------- |
| CREATE                  | Create_priv           | 数据库、表或索引       |
| DROP                    | Drop_priv             | 数据库或表             |
| GRANT OPTION            | Grant_priv            | 数据库、表或保存的程序 |
| REFERENCES              | References_priv       | 数据库或表             |
| ALTER                   | Alter_priv            | 表                     |
| DELETE                  | Delete_priv           | 表                     |
| INDEX                   | Index_priv            | 表                     |
| INSERT                  | Insert_priv           | 表                     |
| SELECT                  | Select_priv           | 表                     |
| UPDATE                  | Update_priv           | 表                     |
| CREATE VIEW             | Create_view_priv      | 视图                   |
| SHOW VIEW               | Show_view_priv        | 视图                   |
| ALTER ROUTINE           | Alter_routine_priv    | 保存的程序             |
| CREATE ROUTINE          | Create_routine_priv   | 保存的程序             |
| EXECUTE                 | Execute_priv          | 保存的程序             |
| FILE                    | File_priv             | 服务器主机上的文件访问 |
| CREATE TEMPORARY TABLES | Create_tmp_table_priv | 服务器管理             |
| LOCK TABLES             | Lock_tables_priv      | 服务器管理             |
| CREATE USER             | Create_user_priv      | 服务器管理             |
| PROCESS                 | Process_priv          | 服务器管理             |
| RELOAD                  | Reload_priv           | 服务器管理             |
| REPLICATION CLIENT      | Repl_client_priv      | 服务器管理             |
| REPLICATION SLAVE       | Repl_slave_priv       | 服务器管理             |
| SHOW DATABASES          | Show_db_priv          | 服务器管理             |
| SHUTDOWN                | Shutdown_priv         | 服务器管理             |
| SUPER                   | Super_priv            | 服务器管理             |

其余的权限用于管理性操作，它使用**mysqladmin**程序或SQL语句实施。下表显示每个管理性权限允许你执行的**mysqladmin**命令：

| **权限** | **权限拥有者允许执行的命令**                                 |
| -------- | ------------------------------------------------------------ |
| RELOAD   | flush-hosts, flush-logs, flush-privileges, flush-status, flush-tables, flush-threads, refresh, reload |
| SHUTDOWN | shutdown                                                     |
| PROCESS  | processlist                                                  |
| SUPER    | kill                                                         |

## :point_right:用户管理

**添加用户**： 可以用两种方式创建MySQL账户：

使用GRANT语句

```sql
 GRANT ALL PRIVILEGES ON *.* TO 'monty'@'localhost'
 IDENTIFIED BY 'some_pass' WITH GRANT OPTION;
```

使用create user

```sql
CREATE USER 'test1'@'localhost' IDENTIFIED BY 'test1';
```

直接操作MySQL授权表

```sql
INSERT INTO mysql.user(Host, User, authentication_string, ssl_cipher, x509_issuer, x509_subject) 
VALUES ('localhost', 'test2', PASSWORD('test2'), '', '', '');
 
 # insert 语句需要刷新数据库 
 FLUSH PRIVILEGES;
```

**限制用户资源**

GRANT语句设置资源限制

```sql
GRANT ALL 
ON customer.* 
TO 'francis'@'localhost'
IDENTIFIED BY 'frank'
WITH MAX_QUERIES_PER_HOUR 20
     MAX_UPDATES_PER_HOUR 10
     MAX_CONNECTIONS_PER_HOUR 5
     MAX_USER_CONNECTIONS 2;
```

**修改密码**

```sql
 SET PASSWORD FOR 'jeffrey'@'%' = PASSWORD('biscuit');
 GRANT USAGE ON *.* TO 'jeffrey'@'%' IDENTIFIED BY 'biscuit'
```

只有root等可以更新mysql数据库的用户可以更改其它用户的密码。如果你没有以匿名用户连接，省略FOR子句便可以更改自己的密码：

```sql
SET PASSWORD = PASSWORD('biscuit');
```

## :point_right:备份与恢复

备份库

```sql
mysqldump -uroot -proot --databases db1 db2 >/tmp/user.sql
mysqldump -uroot -proot --databases db1 --tables a1 a2  >/tmp/db1.sql
mysqlhotcopy db_name /path/to/some/dir
```

备份表

```bash
mysqldump -uroot -proot --databases db1 --tables a1 a2  >/tmp/db1.sql
SELECT * INTO OUTFILE 'file_name' FROM tbl_name。
```

自动恢复

你可以使用**mysqlbinlog**工具来恢复从指定的时间点开始 (例如，从你最后一次备份)直到现在或另一个指定的时间点的数据。

 ```bash
 mysqlbinlog --stop-date="2005-04-20 9:59:59" /var/log/mysql/bin.123456 |mysql -u root -pmypwd
 ```

## 表维护和崩溃恢复

- 要想检查或维护MyISAM表，使用CHECK TABLE或REPAIR TABLE。
- 要想优化MyISAM表，使用OPTIMIZE TABLE。
- 要想分析MyISAM表，使用ANALYZE TABLE。

## :point_right:MySQL日志文件

MySQL有几个不同的日志文件，可以帮助你找出**mysqld**内部发生的事情：

| **日志文件** | **记入文件中的信息类型**                                     |
| ------------ | ------------------------------------------------------------ |
| 错误日志     | 记录启动、运行或停止**mysqld****时出**现的问题。             |
| 查询日志     | 记录建立的客户端连接和执行的语句。                           |
| 更新日志     | 记录更改数据的语句。不赞成使用该日志。                       |
| 二进制日志   | 记录所有更改数据的语句。还用于复制。                         |
| 慢日志       | 记录所有执行时间超过long_query_time秒的所有查询或不使用索引的查询 |

**错误日志**：

可以用--log-error[=*file_name*]选项来指定**mysqld**保存错误日志文件的位置

**通用日志**:

你应该用--log[=*file_name*]或-l [*file_name*]选项启动它。如果没有给定*file_name*的值， 默认名是***host_name*.log**

**二进制日志**:

当用--log-bin[=*file_name*]选项启动时，**mysqld**写入包含所有更新数据的SQL命令的日志文件。如果未给出*file_name*值， 默认名为-bin后面所跟的主机名。你可以用**mysqlbinlog**实用工具检查二进制日志文件。

```bash
mysqlbinlog log-file | mysql -h server_name
```

**慢日志**：

用--log-slow-queries[=*file_name*]选项启动时，**mysqld**写一个包含所有执行时间超过long_query_time秒的SQL语句的日志文件。

# :point_right:SQL优化

[sql优化是一门艺术](https://dev.mysql.com/doc/refman/8.0/en/optimization.html)

## :point_right:优化select语句

- 要 SELECT ... WHERE 加快查询速度，首先要检查的是是否可以添加 **索引** 。
- 隔离并调整查询的任何部分，例如函数调用，这会占用过多时间。 根据查询的结构，可以为结果集中的每一行调用一次函数，甚至可以为表中的每一行调用一次函数，从而大大减轻任何低效率。
- 最大限度地减少 查询中 的 全表扫描 数 ，尤其是对于大表。
- 了解特定于每个表的存储引擎的调优技术，索引技术和配置参数。 双方 InnoDB 并 MyISAM 有两套准则的实现和维持查询高性能
- 使用explain了解过程
- 处理锁定问题

### WHERE子句优化

删除不必要的括号；简化条件

## 优化索引

主键、外键、列、空间

> B树和hash的比较

## 优化数据结构

## 优化InnoDB表

事务管理、只读、磁盘i/o

# :point_right:InnoDB存储引擎

InnoDB（默认） 是一种平衡高可靠性和高性能的通用存储引擎。 在MySQL 8.0中， InnoDB 是默认的MySQL存储引擎。 除非您配置了不同的默认存储引擎，否则发出 CREATE TABLE 不带 ENGINE= 子句的语句会创建 InnoDB 表。

ACID模型、多版本控制、内存缓冲区设置、磁盘结构、I/O、并发

```sql
CREATE TABLE t1 (
    a INT,
    b CHAR (20), 
    PRIMARY KEY (a)
) ENGINE=InnoDB;
```

特征

| 特征                                                         | 支持                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **B树索引**                                                  | 是                                                           |
| **备份/时间点恢复** （在服务器中实现，而不是在存储引擎中实现。） | 是                                                           |
| **群集数据库支持**                                           | 没有                                                         |
| **聚集索引**                                                 | 是                                                           |
| **压缩数据**                                                 | 是                                                           |
| **数据缓存**                                                 | 是                                                           |
| **加密数据**                                                 | 是（通过加密功能在服务器中实现;在MySQL 5.7及更高版本中，支持静态数据表空间加密。） |
| **外键支持**                                                 | 是                                                           |
| **全文搜索索引**                                             | 是（在MySQL 5.6及更高版本中可以使用InnoDB对FULLTEXT索引的支持。） |
| **地理空间数据类型支持**                                     | 是                                                           |
| **地理空间索引支持**                                         | 是（在MySQL 5.7及更高版本中可以使用InnoDB对地理空间索引的支持。） |
| **哈希索引**                                                 | 否（InnoDB在内部利用哈希索引来实现其自适应哈希索引功能。）   |
| **索引缓存**                                                 | 是                                                           |
| **锁定粒度**                                                 | 行                                                           |
| **MVCC**                                                     | 是                                                           |
| **复制支持** （在服务器中实现，而不是在存储引擎中实现。）    | 是                                                           |
| **存储限制**                                                 | 64TB                                                         |
| **T树索引**                                                  | 没有                                                         |
| **交易**                                                     | 是                                                           |
| **更新数据字典的统计信息**                                   | 是                                                           |

# :point_right:MyISAM存储引擎(一般弃用)

```bash
CREATE TABLE t（i INT）ENGINE = MYISAM;
```

`MyISAM` 基于旧的（不再可用） `ISAM` 存储引擎，但有许多有用的扩展。

**MyISAM存储引擎功能**

| 特征                                                         | 支持                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **B树索引**                                                  | 是                                                           |
| **备份/时间点恢复** （在服务器中实现，而不是在存储引擎中实现。） | 是                                                           |
| **群集数据库支持**                                           | 没有                                                         |
| **聚集索引**                                                 | 没有                                                         |
| **压缩数据**                                                 | 是（仅在使用压缩行格式时支持压缩MyISAM表。使用带MyISAM的压缩行格式的表是只读的。） |
| **数据缓存**                                                 | 没有                                                         |
| **加密数据**                                                 | 是（通过加密功能在服务器中实现。）                           |
| **外键支持**                                                 | 没有                                                         |
| **全文搜索索引**                                             | 是                                                           |
| **地理空间数据类型支持**                                     | 是                                                           |
| **地理空间索引支持**                                         | 是                                                           |
| **哈希索引**                                                 | 没有                                                         |
| **索引缓存**                                                 | 是                                                           |
| **锁定粒度**                                                 | 表                                                           |
| **MVCC**                                                     | 没有                                                         |
| **复制支持** （在服务器中实现，而不是在存储引擎中实现。）    | 是                                                           |
| **存储限制**                                                 | 256TB                                                        |
| **T树索引**                                                  | 没有                                                         |
| **交易**                                                     | 没有                                                         |
| **更新数据字典的统计信息**                                   | 是                                                           |

# :point_right: 主从

设置基于二进制日志文件位置的复制。

使用全局事务标识符进行复制

# 建议

运行MySQL时，应尽量遵从下面的指导：

通过SHOW GRANTS语句检查查看谁已经访问了什么。然后使用REVOKE语句删除不再需要的权限。

不要将纯文本密码保存到数据库中。如果你的计算机有安全危险，入侵者可以获得所有的密码并使用它们。相反，应使用MD5()、SHA1()或单向哈希函数。

[function参考](https://dev.mysql.com/doc/refman/8.0/en/functions.html)

[sql语句字典](https://dev.mysql.com/doc/refman/8.0/en/sql-statements.html)
