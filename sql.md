[toc]

# PG



一种好习惯是把关键字写成大写，而名字等用小写：

## 常用

```sql
--查看版本号
SELECT version();
--查看当前时间
SELECT current_date;

--查看数据库大小，不计算索引
select` `pg_size_pretty(pg_database_size(``'mydb'``));
--查看数据库大小，包含索引
select` `pg_size_pretty(pg_total_size(``'mydb'``));
--查看表中索引大小
select` `pg_size_pretty(pg_indexes_size(``'test_1'``));
--查看表大小,不包括索引
select` `pg_size_pretty(pg_relation_size(``'test_1'``)); 
--or
\dt+ test_1
--查看表大小,包括索引
select` `pg_size_pretty(pg_total_relation_size(``'test_1'``)); 
--查看某个模式大小，包括索引。不包括索引可用pg_relation_size
select` `schemaname,round(``sum``(pg_total_relation_size(schemaname||``'.'``||tablename))/1024/1024) ``"Mb"` `from` `pg_tables ``where` `schemaname=``'mysch'` `group` `by` `1;
--查看表空间大小
select` `pg_size_pretty(pg_tablespace_size(``'pg_global'``));
--查看表对应的数据文件
select` `pg_relation_filepath(``'test_1'``);
--切换log日志文件到下一个
select` `pg_rotate_logfile();
--切换日志
select` `pg_switch_xlog();
checkpoint

CREATE TABLE cities (
    name            varchar(80),
    location        point
);
DROP TABLE tablename;

INSERT INTO weather VALUES ('San Francisco', 46, 50, 0.25, '1994-11-27');
INSERT INTO weather (date, city, temp_hi, temp_lo)VALUES ('1994-11-29', 'Hayward', 54, 37);
COPY weather FROM '/home/user/weather.txt';
SELECT DISTINCT city FROM weather;
SELECT * FROM weather INNER JOIN cities ON (weather.city = cities.name);
UPDATE weather SET temp_hi = temp_hi - 2,  temp_lo = temp_lo - 2 WHERE date > '1994-11-28';

DELETE FROM weather WHERE city = 'Hayward';
DELETE FROM tablename;
TRUNCATE TABLE tablename;

SELECT true OR somefunc();
SELECT ... WHERE CASE WHEN x > 0 THEN y/x > 1.5 ELSE false END;

--格式化（可以指定宽度）
format(formatstr text [, formatarg "any" [, ...] ])
%[position][flags][width]type
s格式参数值为简单的字符串。空值作为空字符串对待。
I将参数值作为SQL标识符对待，如果需要，双写它。值为空是错误的。
L引用参数值作为SQL文字。空值用字符串NULL显示，没有引用。
SELECT format('Testing %s, %s, %s, %%', 'one', 'two', 'three');
Result: Testing one, two, three, %

SELECT format('INSERT INTO %I VALUES(%L)', 'Foo bar', E'O\'Reilly');
Result: INSERT INTO "Foo bar" VALUES('O''Reilly')

SELECT format('|%10s|', 'foo');
Result: |       foo|

SELECT format('|%-10s|', 'foo');
Result: |foo       |
SELECT format('|%*2$s|', 'foo', 10, 'bar');
Result: |       bar|

--返回集合的函数
generate_series(start, stop)	int 或 bigint	setof int 或 setof bigint (与参数类型相同)	生成一个数值序列，从start到stop，步长为 1 。
generate_series(start, stop, step)	int 或 bigint	setof int 或 setof bigint (与参数类型相同)	生成一个数值序列，从start到stop，步长为step。
generate_series(start, stop, step interval)	timestamp 或 timestamp with time zone	setof timestamp 或 setof timestamp with time zone (与参数类型相同)	生成一个数值序列，从start到stop，步长为step。

SELECT * FROM generate_series(2,4);
 generate_series
-----------------
               2
               3
               4
(3 rows)

SELECT * FROM generate_series(5,1,-2);
 generate_series
-----------------
               5
               3
               1
             

```

```sql
pg_column_size(any)	int	存储一个指定的数值需要的字节数（可能压缩过）
pg_database_size(oid)	bigint	指定OID的数据库使用的磁盘空间
pg_database_size(name)	bigint	指定名称的数据库使用的磁盘空间
pg_indexes_size(regclass)	bigint	关联指定表OID或表名的表索引的使用总磁盘空间
pg_relation_size(relation regclass, fork text)	bigint	指定OID或名的表或索引，通过指定fork('main', 'fsm' 或'vm')所使用的磁盘空间
pg_relation_size(relation regclass)	bigint	pg_relation_size(..., 'main')的缩写
pg_size_pretty(bigint)	text	Converts a size in bytes expressed as a 64-bit integer into a human-readable format with size units
pg_size_pretty(numeric)	text	把以字节计算的数值转换成一个人类易读的尺寸单位
pg_table_size(regclass)	bigint	指定表OID或表名的表使用的磁盘空间，除去索引（但是包含TOAST，自由空间映射和可视映射）
pg_tablespace_size(oid)	bigint	指定OID的表空间使用的磁盘空间
pg_tablespace_size(name)	bigint	指定名称的表空间使用的磁盘空间
pg_total_relation_size(regclass)	bigint	指定表OID或表名使用的总磁盘空间，包括所有索引和TOAST数据

current_catalog	name	当前数据库名（在SQL标准里叫"catalog"）
current_database()	name	当前数据库名
current_query()	text	当前执行的查询文本，由客户端提交（可能包含多于1句）
current_schema[()]	name	当前模式名
current_schemas(boolean)	name[]	搜索路径中的模式名字，包括可选的隐式模式
current_user	name	当前执行环境下的用户名
inet_client_addr()	inet	连接的远端地址
inet_client_port()	int	连接的远端端口
inet_server_addr()	inet	连接的本地地址
inet_server_port()	int	连接的本地端口
pg_backend_pid()	int	连接到当前会话的服务器进程 ID
pg_conf_load_time()	timestamp with time zone	配置加载时间
pg_is_other_temp_schema(oid)	boolean	是否为另一个会话的临时模式?
pg_listening_channels()	setof text	正在侦听的当前会话的信道名称
pg_my_temp_schema()	oid	会话的临时模式的OID，不存在则为 0
pg_postmaster_start_time()	timestamp with time zone	服务器启动时间
pg_trigger_depth()	int	PostgreSQL 触发器的当前嵌套级别 （如果没有直接或间接的从一个触发器内部调用，那么是0）
session_user	name	会话用户名
user	name	等价于 current_user
version()	text	PostgreSQL 版本信息
```

## 单步模式

 `psql`的`-s` 选项把你置于单步模式，它在向服务器发送每个语句之前暂停。

```sql
psql -s mydb
mydb=> \i basics.sql  --\i命令从指定的文件中读取命令
```

## 聚合函数

在一个行集合上计算`count`(数目), `sum`(总和),`avg`(均值),`max`(最大值), `min`(最小值)的函数

```
SELECT city, max(temp_lo)
    FROM weather
    WHERE city LIKE 'S%'
    GROUP BY city
    HAVING max(temp_lo) < 40;
```

## 视图

```sql
CREATE VIEW myview AS
    SELECT city, temp_lo, temp_hi, prcp, date, location
        FROM weather, cities
        WHERE city = name;

SELECT * FROM myview;
```

## 外键

```sql
CREATE TABLE cities (
        city     varchar(80) primary key,
        location point
);

CREATE TABLE weather (
        city      varchar(80) references cities(city),
        temp_lo   int,
        temp_hi   int,
        prcp      real,
        date      date
);
INSERT INTO weather VALUES ('Berkeley', 45, 53, 0.0, '1994-11-28');
--ERROR:  insert or update on table "weather" violates foreign key constraint "weather_city_fkey"
--DETAIL:  Key (city)=(Berkeley) is not present in table "cities".
```

## 事务

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100.00
    WHERE name = 'Alice';
-- 等等
COMMIT;
--我们决定不做提交(可能是我们刚发现 Alice 的余额是负数)， 那么我们可以发出ROLLBACK而不是COMMIT命令， 那么到目前为止我们的所有更新都会被取消。

PostgreSQL 实际上把每个 SQL 语句当做在一个事务中执行来看待。 如果你没有发出BEGIN命令，那么每个独立的语句都被一个隐含的BEGIN 和(如果成功的话)COMMIT包围。一组包围在BEGIN和COMMIT 之间的语句有时候被称做事务块。

BEGIN;
UPDATE accounts SET balance = balance - 100.00
    WHERE name = 'Alice';
SAVEPOINT my_savepoint;
UPDATE accounts SET balance = balance + 100.00
    WHERE name = 'Bob';
-- 呀！加错钱了，应该用 Wally 的账号
ROLLBACK TO my_savepoint;
UPDATE accounts SET balance = balance + 100.00
    WHERE name = 'Wally';
COMMIT;
```



## 窗口函数

*窗口函数*在和当前行相关的一组表行上执行计算。 这相当于一个可以由聚合函数完成的计算类型。但不同于常规的聚合函数， 使用的窗口函数不会导致行被分组到一个单一的输出行；行保留其独立的身份。

| 函数                                                        | 返回类型           | 描述                                                         |
| ----------------------------------------------------------- | ------------------ | ------------------------------------------------------------ |
| `row_number()`                                              | `bigint`           | 在其分区中的当前行号，从1计                                  |
| `rank()`                                                    | `bigint`           | 有间隔的当前行排名；与它的第一个相同行的`row_number`相同     |
| `dense_rank()`                                              | `bigint`           | 没有间隔的当前行排名；这个函数计数对等组。                   |
| `percent_rank()`                                            | `double precision` | 当前行的相对排名: (`rank` - 1) / (总行数 - 1)                |
| `cume_dist()`                                               | `double precision` | 当前行的相对排名：(前面的行数或与当前行相同的行数)/(总行数)  |
| `ntile(*num_buckets* integer)`                              | `integer`          | 从1到参数值的整数范围，尽可能相等的划分分区。                |
| `lag(*value* any [, *offset* integer [, *default* any ]])`  | `类型同 *value*`   | 计算分区当前行的前`*offset*` 行，返回`*value*` 。如果没有这样的行， 返回`*default*`替代。 `*offset*`和`*default*` 都是当前行计算的结果。如果忽略了，则`*offset*` 默认是1，`*default*`默认是 null。 |
| `lead(*value* any [, *offset* integer [, *default* any ]])` | `类型同*value*`    | 计算分区当前行的后`*offset*`行， 返回`*value*`。如果没有这样的行， 返回`*default*`替代。 `*offset*`和`*default*` 都是当前行计算的结果。如果忽略了，则`*offset*` 默认是1，`*default*`默认是 null。 |
| `first_value(*value* any)`                                  | `类型同*value*`    | 返回窗口第一行的计算`*value*`值。                            |
| `last_value(*value* any)`                                   | `类型同*value*`    | 返回窗口最后一行的计算`*value*`值。                          |
| `nth_value(*value* any, *nth* integer)`                     | `类型同*value*`    | 返回窗口第`*nth*`行的计算 `*value*`值（行从1计数）；没有这样的行则返回 null。 |

```sql
SELECT depname, empno, salary, avg(salary) OVER (PARTITION BY depname) FROM empsalary;
--窗口函数的调用总是包含一个OVER子句，后面直接跟着窗口函数的名称和参数。 这是它在语法上区别于普通函数或聚合功能的地方。 OVER子句决定如何将查询的行进行拆分以便给窗口函数处理。 OVER子句内的PARTITION BY列表指定将行划分成组或分区， 组或分区共享相同的PARTITION BY表达式的值。 对于每一行，窗口函数在和当前行落在同一个分区的所有行上进行计算。你还可以使用窗口函数OVER内的ORDER BY来控制行的顺序。 （ORDER BY窗口甚至不需要与行的输出顺序相匹配。）
SELECT depname, empno, salary, rank() OVER (PARTITION BY depname ORDER BY salary DESC) FROM empsalary;


```



## 继承

```sql
CREATE TABLE cities (
  name       text,
  population real,
  altitude   int     -- (单位是英尺)
);

CREATE TABLE capitals (
  state      char(2)
) INHERITS (cities);

--capitals继承了其父表 cities的所有字段(name,population 和altitude)。字段name的类型text是 PostgreSQL用于变长字符串的固有类型。州首府有一个额外的字段 state显示其所处的州。

SELECT name, altitude
  FROM cities
  WHERE altitude > 500;
  
 SELECT name, altitude
    FROM ONLY cities
    WHERE altitude > 500;   
 --cities前面的ONLY指示系统只对cities 表运行查询，而不包括继承级别中低于cities的表。 许多我们已经讨论过的命令—SELECT, UPDATE和 DELETE—都支持这个ONLY表示法。
 
```

## 函数

```sql
CREATE FUNCTION dept(text) RETURNS dept
    AS $$ SELECT * FROM dept WHERE name = $1 $$
    LANGUAGE SQL;
   
 --在函数被调用的时候这里的$1将引用第一个参数。
 
 CREATE FUNCTION concat_lower_or_upper(a text, b text, uppercase boolean DEFAULT false)
RETURNS text
AS
$$
 SELECT CASE
        WHEN $3 THEN UPPER($1 || ' ' || $2)
        ELSE LOWER($1 || ' ' || $2)
        END;
$$
LANGUAGE SQL IMMUTABLE STRICT;

-----------------------------------------------------
SELECT concat_lower_or_upper(a := 'Hello', b := 'World');
 concat_lower_or_upper 
-----------------------
 hello world
 
 SELECT concat_lower_or_upper(a := 'Hello', b := 'World', uppercase := true);
 concat_lower_or_upper 
-----------------------
 HELLO WORLD
(1 row)

SELECT concat_lower_or_upper(a := 'Hello', uppercase := true, b := 'World');
 concat_lower_or_upper 
-----------------------
 HELLO WORLD
 
 --混合
 SELECT concat_lower_or_upper('Hello', 'World', uppercase := true);
 concat_lower_or_upper 
-----------------------
 HELLO WORLD
 
 
```

## 聚合表达式

```sql
SELECT array_agg(a ORDER BY b DESC) FROM table;
-- count(*)生成输入行的总数；count(f1)生成 f1不为 NULL 的输入行数，因为count忽略NULL； count(distinct f1)生成f1唯一且非 NULL 的行数。
```

## 类型转换

```sql
CAST ( expression AS type )
expression::type
--CAST语法遵循 SQL 标准；::语法是PostgreSQL历史用法。
typename ( expression )

```

## 排序规则表达式

```sql
expr COLLATE collation
SELECT a, b, c FROM tbl WHERE ... ORDER BY a COLLATE "C";
```

## 数据定义

```sql

CREATE TABLE products (
    product_no integer DEFAULT nextval('products_product_no_seq'),
    ...
);
```

## 缺省值

```sql
CREATE TABLE products (
    product_no integer,
    name text,
    price numeric DEFAULT 9.99
);

CREATE TABLE products (
    product_no integer DEFAULT nextval('products_product_no_seq'),
    ...
);

CREATE TABLE products (
    product_no SERIAL,
    ...
);

```

## 约束

```sql
CREATE TABLE products (
    product_no integer,
    name text,
    price numeric CHECK (price > 0)
);
-- 一个检查约束由一个关键字CHECK后面跟一个放在圆括弧里的表达式组成。
-- 你还可以给这个约束取一个独立的名字。这样就可以令错误消息更清晰,要声明一个命名约束，使用关键字CONSTRAINT后面跟一个标识符(作为名字)， 然后再跟约束定义。
CREATE TABLE products (
    product_no integer,
    name text,
    price numeric CONSTRAINT positive_price CHECK (price > 0)
);

-- 非空约束
CREATE TABLE products (
    product_no integer NOT NULL,
    name text NOT NULL,
    price numeric
);

--  唯一约束
CREATE TABLE products (
    product_no integer UNIQUE,
    name text,
    price numeric
);

CREATE TABLE products (
    product_no integer,
    name text,
    price numeric,
    UNIQUE (product_no)
);

-- 主键 （下面两个等价）
CREATE TABLE products (
    product_no integer UNIQUE NOT NULL,
    name text,
    price numeric
);
CREATE TABLE products (
    product_no integer PRIMARY KEY,
    name text,
    price numeric
);

-- 外键 外键约束声明一个字段(或者一组字段)的数值必须匹配另外一个表中出现的数值。 我们把这个行为称为两个相关表之间的参照完整性。
CREATE TABLE orders (
    order_id integer PRIMARY KEY,
    product_no integer REFERENCES products (product_no),--因为如果缺少字段列表的话，就会引用被引用表的主键。
    quantity integer
);

CREATE TABLE t1 (
  a integer PRIMARY KEY,
  b integer,
  c integer,
  FOREIGN KEY (b, c) REFERENCES other_table (c1, c2)
);  --被约束的字段数目和类型需要和被引用字段数目和类型一致。

CREATE TABLE products (
    product_no integer PRIMARY KEY,
    name text,
    price numeric
);

CREATE TABLE orders (
    order_id integer PRIMARY KEY,
    shipping_address text,
    ...
);

CREATE TABLE order_items (
    product_no integer REFERENCES products ON DELETE RESTRICT,
    order_id integer REFERENCES orders ON DELETE CASCADE,
    quantity integer,
    PRIMARY KEY (product_no, order_id)
);

限制和级联删除是两种最常见的选项。RESTRICT禁止删除被引用的行。 NO ACTION的意思是如果在检查约束的时候还存在任何引用行，则抛出错误； 如果你不声明任何东西，那么它就是缺省的行为。这两个选择的实际区别是：NO ACTION 允许约束检查推迟到事务的晚些时候，而RESTRICT不行。CASCADE 声明在删除一个被引用的行的时候，所有引用它的行也会被自动删除掉。在外键字段上的动作还有两个选项： SET NULL和SET DEFAULT，它们导致在被引用行删除的时候， 将引用它们的字段分别设置为 NULL 和缺省值。请注意这些选项并不能让你逃脱被观察和约束的境地。 比如，如果一个动作声明SET DEFAULT，但是缺省值并不能满足外键约束， 那么该动作就会失败。
与ON DELETE类似的还有ON UPDATE选项， 它是在被引用字段修改(更新)的时候调用的，可用的动作是一样的。 在这种情况下，CASCADE意味着被引用字段的更新后的值， 应该被拷贝到引用行中。
通常地，如果一个引用行的任意引用字段为null，那么这个引用行不必满足外键约束。 如果外键声明中添加了MATCH FULL，引用行只有在所有的引用字段都是null时， 才能逃避满足约束（所以null和non-null值的混合肯定不能满足MATCH FULL约束）。 如果你不想引用行能够避免满足外键约束，那么声明引用行为NOT NULL。


```

## 修改表

```sql
-- 增加字段
ALTER TABLE products ADD COLUMN description text;
ALTER TABLE products ADD COLUMN description text CHECK (description <> '');

-- 删除字段
ALTER TABLE products DROP COLUMN description; -- 如果这个字段被另一个表的外键约束所引用，PostgreSQL 则不会隐含地删除该约束。你可以通过使用CASCADE指明删除任何依赖该字段的东西：
ALTER TABLE products DROP COLUMN description CASCADE;

--增加约束
ALTER TABLE products ADD CHECK (name <> '');
ALTER TABLE products ADD CONSTRAINT some_name UNIQUE (product_no);
ALTER TABLE products ADD FOREIGN KEY (product_group_id) REFERENCES product_groups;
要增加一个不能写成表约束的非空约束
ALTER TABLE products ALTER COLUMN product_no SET NOT NULL;

-- 删除约束
ALTER TABLE products DROP CONSTRAINT some_name;
ALTER TABLE products ALTER COLUMN product_no DROP NOT NULL;

-- 改变字段的缺省值
ALTER TABLE products ALTER COLUMN price SET DEFAULT 7.77;
删除缺省值
ALTER TABLE products ALTER COLUMN price DROP DEFAULT;

-- 修改字段的数据类型
ALTER TABLE products ALTER COLUMN price TYPE numeric(10,2);

-- 重命名字段
ALTER TABLE products RENAME COLUMN product_no TO product_number;

--重命名表
ALTER TABLE products RENAME TO items;

```

## 权限

```sql
--当创建一个数据库对象时，它就被赋予了所有者。这个所有者通常是执行创建语句的角色。 对大多数类型的对象，初始状态只有其所有者（或者超级管理员）可以对它做任何事情。 要允许其他角色使用它，必须要经过权限授予。

--使用GRANT命令赋予权限。例如，如果joe是一个已经存在的用户， 而accounts是一个已经存在的表，更新表的权限可以用下面的命令赋予：有好多种不同的权限：SELECT, INSERT, UPDATE, DELETE, TRUNCATE, REFERENCES, TRIGGER, CREATE, CONNECT, TEMPORARY, EXECUTE, 和 USAGE 。

GRANT UPDATE ON accounts TO joe;

--名为PUBLIC的特殊"用户"可以用于将权限赋予系统中的所有用户。 可以使用REVOKE命令撤销权限：
REVOKE ALL ON accounts FROM PUBLIC;

```

## 模式(重要)

```sql
-- 一个数据库包含一个或多个已命名的模式，模式又包含表。模式还可以包含其它对象， 包括数据类型、函数、操作符等。
CREATE SCHEMA myschema;

-- 访问
schema.table
database.schema.table

-- 创建
CREATE TABLE myschema.mytable (
 ...
);

-- 删除
DROP SCHEMA myschema;
DROP SCHEMA myschema CASCADE;

-- Public 模式
-- 在前面的小节里，我们没有声明任何模式名字就创建了表。缺省时， 这样的表(以及其它对象)都自动放到一个叫做"public"的模式中去了。 每个新数据库都包含一个这样的模式。因此，下面的命令是等效的
CREATE TABLE products ( ... );
CREATE TABLE public.products ( ... );

```

## 查询

```sql
条件连接可能的类型是：
INNER JOIN

	内连接。对于 T1 中的每一行 R1 ，如果能在 T2 中找到一个或多个满足连接条件的行， 那么这些满足条件的每一行都在连接表中生成一行。
LEFT OUTER JOIN
	左外连接。首先执行一次内连接。然后为每一个 T1 中无法在 T2 中找到匹配的行生成一行， 该行中对应 T2 的列用 NULL 补齐。因此，生成的连接表里总是包含来自 T1 里的每一行至少一个副本。
	
RIGHT OUTER JOIN
	右外连接。首先执行一次内连接。然后为每一个 T2 中无法在 T1 中找到匹配的行生成一行， 该行中对应 T1 的列用 NULL 补齐。因此，生成的连接表里总是包含来自 T2 里的每一行至少一个副本。
	
FULL OUTER JOIN
	全连接。首先执行一次内连接。然后为每一个 T1 与 T2 中找不到匹配的行生成一行， 该行中无法匹配的列用 NULL 补齐。因此，生成的连接表里无条件地包含 T1 和 T2 里的每一行至少一个副本。
```

## 组合查询

```sql
-- 可以对两个查询的结果进行集合操作(并、交、差)。语法是：
query1 UNION [ALL] query2
query1 INTERSECT [ALL] query2
query1 EXCEPT [ALL] query2
```

## 行排序

```sql
SELECT select_list
    FROM table_expression
    ORDER BY sort_expression1 [ASC | DESC] [NULLS { FIRST | LAST }]
             [, sort_expression2 [ASC | DESC] [NULLS { FIRST | LAST }] ...]

--NULLS FIRST和NULLS LAST选项可以决定在排序操作中在 non-null 值之前还是之后。默认情况下，空值大于任何非空值；也就是说，DESC 排序默认是NULLS FIRST，否则为NULLS LAST。
```

## LIMIT和OFFSET

```sql
SELECT select_list
    FROM table_expression
    [ ORDER BY ... ]
    [ LIMIT { number | ALL } ] [ OFFSET number ]
   
--如果给出了一个 LIMIT 计数，那么将返回不超过该数字的行(也可能更少些， 因为可能查询本身生成的总行数就比较少)。LIMIT ALL和省略LIMIT子句是一样的。
--OFFSET指明在开始返回行之前忽略多少行。OFFSET 0和省略 OFFSET子句是一样的，LIMIT NULL和省略LIMIT子句 是一样的。如果OFFSET和LIMIT都出现了，那么在计算返回的LIMIT 之前先忽略OFFSET指定的行数。
```

## VALUES列表

```sql
VALUES ( expression [, ...] ) [, ...]
=> SELECT * FROM (VALUES (1, 'one'), (2, 'two'), (3, 'three')) AS t (num,letter);
 num | letter
-----+--------
   1 | one
   2 | two
   3 | three
(3 rows)
--带有表达式列表的VALUES和下面的语句等价
SELECT select_list FROM table_expression
```

## with 查询 (通用表表达式)

```sql
--WITH提供了一种在更大的查询中编写辅助语句的方式。 这个通常称为通用表表达式或CTEs的辅助语句可以认为是定义只存在于一个查询中的临时表。 每个WITH子句中的辅助语句可以是一个SELECT,INSERT, UPDATE 或 DELETE；并且WITH子句本身附加到的初级语句可以是一个SELECT, INSERT, UPDATE或DELETE。

WITH regional_sales AS (
        SELECT region, SUM(amount) AS total_sales
        FROM orders
        GROUP BY region
     ), top_regions AS (
        SELECT region
        FROM regional_sales
        WHERE total_sales > (SELECT SUM(total_sales)/10 FROM regional_sales)
     )
SELECT region,
       product,
       SUM(quantity) AS product_units,
       SUM(amount) AS product_sales
FROM orders
WHERE region IN (SELECT region FROM top_regions)
GROUP BY region, product;
-- 外层SELECT将返回更新了的数据。
WITH t AS (
    UPDATE products SET price = price * 1.05
    RETURNING *
)
SELECT * FROM t;
```

## PG数据类型

| 名字                                        | 别名                     | 描述                           |
| ------------------------------------------- | ------------------------ | ------------------------------ |
| `bigint`                                    | `int8`                   | 有符号8字节整数                |
| `bigserial`                                 | `serial8`                | 自增8字节整数                  |
| `bit [ (*n*) ]`                             |                          | 定长位串                       |
| `bit varying [ (*n*) ]`                     | `varbit`                 | 可变长位串                     |
| `boolean`                                   | `bool`                   | 逻辑布尔值(真/假)              |
| `box`                                       |                          | 平面上的矩形                   |
| `bytea`                                     |                          | 二进制数据("字节数组")         |
| `character varying [ (*n*) ]`               | `varchar [ (*n*) ]`      | 可变长字符串                   |
| `character [ (*n*) ]`                       | `char [ (*n*) ]`         | 定长字符串                     |
| `cidr`                                      |                          | IPv4 或 IPv6 网络地址          |
| `circle`                                    |                          | 平面上的圆                     |
| `date`                                      |                          | 日历日期(年, 月, 日)           |
| `double precision`                          | `float8`                 | 双精度浮点数(8字节)            |
| `inet`                                      |                          | IPv4 或 IPv6 主机地址          |
| `integer`                                   | `int`, `int4`            | 有符号 4 字节整数              |
| `interval [ *fields* ] [ (*p*) ]`           |                          | 时间间隔                       |
| `line`                                      |                          | 平面上的无限长直线             |
| `lseg`                                      |                          | 平面上的线段                   |
| `macaddr`                                   |                          | MAC (Media Access Control)地址 |
| `money`                                     |                          | 货币金额                       |
| `numeric [ (*p*, *s*) ]`                    | `decimal [ (*p*, *s*) ]` | 可选精度的准确数值数据类型     |
| `path`                                      |                          | 平面上的几何路径               |
| `point`                                     |                          | 平面上的点                     |
| `polygon`                                   |                          | 平面上的封闭几何路径           |
| `real`                                      | `float4`                 | 单精度浮点数(4 字节)           |
| `smallint`                                  | `int2`                   | 有符号 2 字节整数              |
| `smallserial`                               | `serial2`                | 自增 2 字节整数                |
| `serial`                                    | `serial4`                | 自增 4 字节整数                |
| `text`                                      |                          | 可变长字符串                   |
| `time [ (*p*) ] [ without time zone ]`      |                          | 一天中的时刻(无时区)           |
| `time [ (*p*) ] with time zone`             | `timetz`                 | 一天中的时刻，含时区           |
| `timestamp [ (*p*) ] [ without time zone ]` |                          | 日期与时刻(无时区)             |
| `timestamp [ (*p*) ] with time zone`        | `timestamptz`            | 日期与时刻，含时区             |
| `tsquery`                                   |                          | 文本检索查询                   |
| `tsvector`                                  |                          | 文本检索文档                   |
| `txid_snapshot`                             |                          | 用户级别的事务ID快照           |
| `uuid`                                      |                          | 通用唯一标识符                 |
| `xml`                                       |                          | XML 数据                       |
| `json`                                      |                          | JSON 数据                      |

## 序列号类型

```sql
--smallserial,serial和bigserial类型不是真正的类型， 只是为在表中创建唯一标识做的概念上的便利。类似其它一些数据库中的AUTO_INCREMENT 属性。
CREATE TABLE tablename (
    colname SERIAL
);

-- 等价于
CREATE SEQUENCE tablename_colname_seq;
CREATE TABLE tablename (
    colname integer NOT NULL DEFAULT nextval('tablename_colname_seq')
);
ALTER SEQUENCE tablename_colname_seq OWNED BY tablename.colname;
```

## 文本搜索类型

```sql
-- tsvector
tsvector的值是一个无重复值的lexemes排序列表，在输入的同时会自动排序和消除重复
SELECT 'a fat cat sat on a mat and ate a fat rat'::tsvector;
                      tsvector
----------------------------------------------------
 'a' 'and' 'ate' 'cat' 'fat' 'mat' 'on' 'rat' 'sat'
 
SELECT 'a:1 fat:2 cat:3 sat:4 on:5 a:6 mat:7 and:8 ate:9 a:10 fat:11 rat:12'::tsvector;
                                  tsvector
-------------------------------------------------------------------------------
 'a':1,6,10 'and':8 'ate':9 'cat':3 'fat':2,11 'mat':7 'on':5 'rat':12 'sat':4
 
 --tsquery存储用于检索的词汇，并且使用布尔操作符 &(AND)，|(OR)和!(NOT) 来组合它们。括号用来强调操作符的分组
 SELECT 'fat & rat'::tsquery;
    tsquery    
---------------
 'fat' & 'rat'

SELECT 'fat & (rat | cat)'::tsquery;
          tsquery          
---------------------------
 'fat' & ( 'rat' | 'cat' )

SELECT 'fat & rat & ! cat'::tsquery;
        tsquery         
------------------------
 'fat' & 'rat' & !'cat'
```

## Arrays

```sql
CREATE TABLE sal_emp (
    name            text,
    pay_by_quarter  integer[],
    schedule        text[][]
);

INSERT INTO sal_emp
    VALUES ('Bill',
    '{10000, 10000, 10000, 10000}',
    '{{"meeting", "lunch"}, {"training", "presentation"}}');

-- 访问数组 PostgreSQL 缺省使用以 1 为基的数组习惯，也就是说，一个n 元素的数组从array[1]开始，到array[n]结束。
SELECT name FROM sal_emp WHERE pay_by_quarter[1] <> pay_by_quarter[2];

--修改数组
UPDATE sal_emp SET pay_by_quarter = '{25000,25000,27000,27000}'
    WHERE name = 'Carol';
UPDATE sal_emp SET pay_by_quarter = ARRAY[25000,25000,27000,27000]
    WHERE name = 'Carol';
    --更新某个元素
UPDATE sal_emp SET pay_by_quarter[4] = 15000
    WHERE name = 'Bill';
    --更新某个区间
UPDATE sal_emp SET pay_by_quarter[1:2] = '{27000,27000}'
    WHERE name = 'Carol';

-- array 连接
SELECT ARRAY[1,2] || ARRAY[3,4];
 ?column?
-----------
 {1,2,3,4}
(1 row)

SELECT ARRAY[5,6] || ARRAY[[1,2],[3,4]];
      ?column?
---------------------
 {{5,6},{1,2},{3,4}}

--在数组中检索
SELECT * FROM sal_emp WHERE pay_by_quarter[1] = 10000 OR
                            pay_by_quarter[2] = 10000 OR
                            pay_by_quarter[3] = 10000 OR
                            pay_by_quarter[4] = 10000;
                            
SELECT * FROM sal_emp WHERE 10000 = ANY (pay_by_quarter);
SELECT * FROM sal_emp WHERE 10000 = ALL (pay_by_quarter);
SELECT * FROM sal_emp WHERE pay_by_quarter && ARRAY[10000];
```

## 对象标识符类型

PostgreSQL在内部使用对象标识符(OID)作为各种系统表的主键。 同时，系统不会给用户创建的表增加一个 OID 系统字段(除非在建表时声明了`WITH OIDS` 或者配置参数[default_with_oids](http://www.postgres.cn/docs/9.3/runtime-config-compatible.html#GUC-DEFAULT-WITH-OIDS)设置为开启)。`oid` 类型代表一个对象标识符。除此以外`oid`还有几个别名：`regproc`, `regprocedure`, `regoper`, `regoperator`, `regclass`, `regtype` `regconfig`, 和`regdictionary`。 <font color = "red">OID 最好只是用于系统表。</font>

| 名字            | 引用           | 描述               | 数值例子                                  |
| --------------- | -------------- | ------------------ | ----------------------------------------- |
| `oid`           | 任意           | 数字化的对象标识符 | `564182`                                  |
| `regproc`       | `pg_proc`      | 函数名字           | `sum`                                     |
| `regprocedure`  | `pg_proc`      | 带参数类型的函数   | `sum(int4)`                               |
| `regoper`       | `pg_operator`  | 操作符名           | `+`                                       |
| `regoperator`   | `pg_operator`  | 带参数类型的操作符 | `*(integer,integer)` 或 `-(NONE,integer)` |
| `regclass`      | `pg_class`     | 关系名             | `pg_type`                                 |
| `regtype`       | `pg_type`      | 数据类型名         | `integer`                                 |
| `regconfig`     | `pg_ts_config` | 文本搜索配置       | `english`                                 |
| `regdictionary` | `pg_ts_dict`   | 文本搜索字典       | `simple`                                  |

```sql
--  检查和一个表mytable相关的pg_attribute行，我们可以这样写:
SELECT * FROM pg_attribute WHERE attrelid = 'mytable'::regclass;

	-- 而不用 
SELECT * FROM pg_attribute
  WHERE attrelid = (SELECT oid FROM pg_class WHERE relname = 'mytable');
```

## 伪类型

PostgreSQL类型系统包含一系列特殊用途的条目， 它们按照类别来说叫做*伪类型*。伪类型不能作为字段的数据类型， 但是它可以用于声明一个函数的参数或者结果类型。 伪类型在一个函数不只是简单地接受并返回某种SQL 数据类型的情况下很有用。

| 名字               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| `any`              | 表示一个函数接受任何输入数据类型。                           |
| `anyelement`       | 表示一个函数接受任何数据类型 (参阅[第 35.2.5 节](http://www.postgres.cn/docs/9.3/extend-type-system.html#EXTEND-TYPES-POLYMORPHIC))。 |
| `anyarray`         | 表示一个函数接受任意数组数据类型 (参阅[第 35.2.5 节](http://www.postgres.cn/docs/9.3/extend-type-system.html#EXTEND-TYPES-POLYMORPHIC))。 |
| `anynonarray`      | 表示一个函数接受任意非数组数据类型 (参阅[第 35.2.5 节](http://www.postgres.cn/docs/9.3/extend-type-system.html#EXTEND-TYPES-POLYMORPHIC))。 |
| `anyenum`          | 表示一个函数接受任意枚举数据类型 (参阅[第 35.2.5 节](http://www.postgres.cn/docs/9.3/extend-type-system.html#EXTEND-TYPES-POLYMORPHIC) 和 [第 8.7 节](http://www.postgres.cn/docs/9.3/datatype-enum.html))。 |
| `anyrange`         | 表示一个函数接受任意范围数据类型 (参阅 [第 35.2.5 节](http://www.postgres.cn/docs/9.3/extend-type-system.html#EXTEND-TYPES-POLYMORPHIC) 和 [第 8.17 节](http://www.postgres.cn/docs/9.3/rangetypes.html))。 |
| `cstring`          | 表示一个函数接受或者返回一个空结尾的 C 字符串。              |
| `internal`         | 表示一个函数接受或者返回一种服务器内部的数据类型。           |
| `language_handler` | 一个过程语言调用处理器声明为返回`language_handler`。         |
| `fdw_handler`      | 一个外部数据封装器声明为返回`fdw_handler`。                  |
| `record`           | 标识一个函数返回一个未声明的行类型。                         |
| `trigger`          | 一个触发器函数声明为返回`trigger`。                          |
| `void`             | 表示一个函数不返回数值。                                     |
| `opaque`           | 一个已经过时的类型，以前用于所有上面这些用途。               |

## 函数与操作符

```sql
--逻辑操作符
AND,OR,NOT

--比较操作符
< , > , != , >= , <= ,<>
a BETWEEN x AND y   =>	 a >= x AND a <= y
a NOT BETWEEN x AND y 	=>	a < x OR a > y


--数学操作符
操作符	描述	例子	结果
+	加	2 + 3	5
-	减	2 - 3	-1
*	乘	2 * 3	6
/	除(整数除法将截断结果)	4 / 2	2
%	模(求余)	5 % 4	1
^	幂(指数运算)	2.0 ^ 3.0	8
|/	平方根	|/ 25.0	5
||/	立方根	||/ 27.0	3
!	阶乘	5 !	120
!!	阶乘(前缀操作符)	!! 5	120
@	绝对值	@ -5.0	5
&	二进制 AND	91 & 15	11
|	二进制 OR	32 | 3	35
#	二进制 XOR	17 # 5	20
~	二进制 NOT	~1	-2
<<	二进制左移	1 << 4	16
>>	二进制右移	8 >> 2	2

--字符串
string || string	text	字符串连接	'Post' || 'greSQL'	PostgreSQL
string || non-string 或 non-string || string	text	带有一个非字符串输入的字符串连接	'Value: ' || 42	Value: 42
bit_length(string)	int	字符串的位	bit_length('jose')	32
char_length(string) 或 character_length(string)	int	字符串中的字符个数	char_length('jose')	4
lower(string)	text	把字符串转化为小写	lower('TOM')	tom
octet_length(string)	int	字符串中的字节数	octet_length('jose')	4
overlay(string placing string from int [for int])	text	替换子字符串	overlay('Txxxxas' placing 'hom' from 2 for 4)	Thomas
position(substring in string)	int	指定子字符串的位置	position('om' in 'Thomas')	3
substring(string [from int] [for int])	text	截取子字符串	substring('Thomas' from 2 for 3)	hom
substring(string from pattern)	text	截取匹配POSIX正则表达式的子字符串。参阅第 9.7 节 获取更多关于模式匹配的信息。	substring('Thomas' from '...$')	mas
substring(string from pattern for escape)	text	截取匹配SQL正则表达式的子字符串。 参阅第 9.7 节获取更多关于模式匹配的信息。	substring('Thomas' from '%#"o_a#"_' for '#')	oma
trim([leading | trailing | both] [characters] from string)	text	从字符串string的开头/结尾/两边删除只包含 characters中字符 (缺省是空白)的最长的字符串	trim(both 'x' from 'xTomxx')	Tom
upper(string)	text	把字符串转化为大写	upper('tom')	TOM

--支持枚举函数
CREATE TYPE rainbow AS ENUM ('red', 'orange', 'yellow', 'green', 'blue', 'purple');

```

## 匹配模式

```sql
-- like  在pattern 里的下划线(_)匹配任何单个字符；而一个百分号(%) 匹配零或多个任何序列。
string LIKE pattern [ESCAPE escape-character]
string NOT LIKE pattern [ESCAPE escape-character]


--SIMILAR TO 正则表达式
string SIMILAR TO pattern [ESCAPE escape-character]
string NOT SIMILAR TO pattern [ESCAPE escape-character]
|表示选择(两个候选之一)
*表示重复前面的项零次或更多次
+表示重复前面的项一次或更多次
?表示重复前面的项零次或一次
{m}表示重复前面的项正好m次
{m,}表示重复前面的项m或更多次
{m,n} 表示重复前面的项至少m次，最多不超过n次
Parentheses ()把项组合成一个逻辑项
[...] 声明一个字符类，只在POSIX正则表达式中
'abc' SIMILAR TO 'abc'      true
'abc' SIMILAR TO 'a'        false
'abc' SIMILAR TO '%(b|d)%'  true
'abc' SIMILAR TO '(b|c)%'   false

--返回子串
substring('foobar' from '%#"o_b#"%' for '#')   oob
substring('foobar' from '#"o_b#"%' for '#')    NULL

--POSIX 正则表达式
操作符	描述	例子
~	匹配正则表达式，大小写相关	'thomas' ~ '.*thomas.*'
~*	匹配正则表达式，大小写无关	'thomas' ~* '.*Thomas.*'
!~	不匹配正则表达式，大小写相关	'thomas' !~ '.*Thomas.*'
!~*	不匹配正则表达式，大小写无关	'thomas' !~* '.*vadim.*'
'abc' ~ 'abc'    true
'abc' ~ '^a'     true
'abc' ~ '(b|d)'  true
'abc' ~ '^(b|c)' false
substring('foobar' from 'o.b')     oob
substring('foobar' from 'o(.)b')   o
regexp_replace('foobarbaz', 'b..', 'X')  	fooXbaz
regexp_replace('foobarbaz', 'b..', 'X', 'g')	 fooXX
regexp_replace('foobarbaz', 'b(..)', E'X\\1Y', 'g')		fooXarYXazY


```

## 延时执行

```sql
pg_sleep(seconds)
SELECT pg_sleep(1.5);
```

## 序列操作函数

本节描述用于操作*序列对象*的函数， 也叫序列生成器或者就叫序列。序列对象都是用[CREATE SEQUENCE](http://www.postgres.cn/docs/9.3/sql-createsequence.html) 创建的特殊的单行表。

| 函数                                | 返回类型 | 描述                                        |
| ----------------------------------- | -------- | ------------------------------------------- |
| `currval(regclass)`                 | `bigint` | 返回最近一次用`nextval`获取的指定序列的数值 |
| `lastval()`                         | `bigint` | 返回最近一次用`nextval`获取的任意序列的数值 |
| `nextval(regclass)`                 | `bigint` | 递增序列并返回新值                          |
| `setval(regclass, bigint)`          | `bigint` | 设置序列的当前数值                          |
| `setval(regclass, bigint, boolean)` | `bigint` | 设置序列的当前数值以及`is_called`标志       |

## 条件表达式

```sql
--CASE表达式是一种通用的条件表达式，类似于其它编程语言中的 if/else 语句。
CASE expression
    WHEN value THEN result
    [WHEN ...]
    [ELSE result]
END

SELECT a,
       CASE WHEN a=1 THEN 'one'
            WHEN a=2 THEN 'two'
            ELSE 'other'
       END
    FROM test;

--需要了解一下
COALESCE、NULLIF 、GREATEST and LEAST 
```

## 子查询表达式

```sql
-- EXISTS   参数是一个任意的SELECT语句，或者说子查询。 系统对子查询进行运算以判断它是否返回行。如果它至少返回一行，那么EXISTS的结果就为 "真"；如果子查询没有返回任何行，那么EXISTS的结果是"假"。
EXISTS (subquery)

SELECT col1
FROM tab1
WHERE EXISTS (SELECT 1 FROM tab2 WHERE col2 = tab1.col2);

-- IN / NOT IN  左边表达式对子查询结果的每一行进行一次计算和比较。如果找到任何相等的子查询行， 则IN结果为"真"。
expression IN (subquery)
row_constructor IN (subquery)  -- 构造器
expression NOT IN (subquery)
row_constructor NOT IN (subquery) 

-- ANY/SOME/ALL    SOME是ANY的同意词。IN等效于= ANY。
expression operator ANY (subquery)
expression operator SOME (subquery)
row_constructor operator ANY (subquery)
row_constructor operator SOME (subquery)
expression operator ALL (subquery)
row_constructor operator ALL (subquery)

-- 逐行比较
row_constructor operator (subquery) 	--左边是一个行构造器(如第 4.2.13 节所述)， 右边是一个圆括弧括起来的子查询，它必须返回和左边行构造器一样多的字段。

```

## 触发器

```sql
-- 触发器是数据库的回调函数，它会在指定的数据库事件发生时自动执行/调用
可以在下面几种情况下触发：
    在执行操作之前（在检查约束并尝试插入、更新或删除之前）。
    在执行操作之后（在检查约束并插入、更新或删除完成之后）。
    更新操作（在对一个视图进行插入、更新、删除时）。
    

CREATE [ CONSTRAINT ] TRIGGER name 
{ BEFORE | AFTER | INSTEAD OF } { event [ OR ... ]}
ON table_name
[ FROM referenced_table_name ]
{ NOT DEFERRABLE | [ DEFEREABLE ] { IINITIALLY IMMEDIATE | INITIALLY DEFERED} }
FOR [ EACH ] { ROW | STATEMENT }
[ WHEN { condition }]
EXECUTE PROCEDURE function_name ( arguments )

在这里，event_name 可以是在所提到的表 table_name 上的 INSERT、DELETE 和 UPDATE 数据库操作。您可以在表名后选择指定 FOR EACH ROW。

--创建方法 CREATE OR REPLEASE FUNCTION student_delete_trigger_fun()
returns trigger as $$
begin
    delete from score where student_no = old.student_no;
    return old;
end;
$$
language plpgsql;

--创建触发器
CREATE TRIGGER delete_student_trigger
after delete on student
for each row execute procedure student_delete_trigger_fun(); 
```

## 索引类型

```sql
-- B-tree 
	适合处理那些能够按顺序存储的数据之上的等于和范围查询。 特别是在一个建立了索引的字段涉及到使用(<、<=、=、>=、>)

-- hash
	Hash 索引只能处理简单的等于比较。当一个索引了的列涉及到使用= 操作符进行比较的时候，查询规划器会考虑使用 Hash 索引。
	CREATE INDEX name ON table USING hash (column);

-- GiST
	 GiST 索引不是单独一种索引类型，而是一种架构，可以在这种架构上实现很多不同的索引策略。 因此，可以使用 GiST 索引的操作符高度依赖于索引策略(操作符类)。 作为示例，PostgreSQL的标准发布中包含用于二维几何数据类型的 GiST 操作符类，它支持（<<、&<、&>、>>、<<|、&<|、|&>、|>>、@>、<@、~=、&&）


-- 索引与order by
-- 唯一索引
	CREATE UNIQUE INDEX name ON table (column [, ...]);
-- SELECT * FROM people WHERE (first_name || ' ' || last_name) = 'John Smith';
	CREATE INDEX people_names ON people ((first_name || ' ' || last_name)); --REATE INDEX命令的语法通常要求在索引表达式周围书写圆括弧

-- 部分唯一索引
	CREATE INDEX orders_unbilled_index ON orders (order_nr)
   		WHERE billed is not true;
   	
   	CREATE TABLE tests (
        subject text,
        target text,
        success boolean,
        ...
	);
    CREATE UNIQUE INDEX tests_success_constraint ON tests (subject, target)
        WHERE success;
 -- 为了加速文本搜索，我们可以创建GIN索引
 	CREATE INDEX pgweb_idx ON pgweb USING gin(to_tsvector('english', body));
```

## 并发控制

```sql
--  事务隔离
各个级别不应该发生的现象是：
    脏读
   		一个事务读取了另一个未提交事务写入的数据。
    不可重复读
    	一个事务重新读取前面读取过的数据，发现该数据已经被另一个已经提交的事务修改。
    幻读
    	一个事务重新执行一个查询，返回符合查询条件的行的集合，发现满足查询条件的行的集合因为其它最近提交的事务而发生了改变。

-- 隔离级别	脏读	不可重复读	幻读
    读未提交	可能	可能	可能
    读已提交	不可能	可能	可能
    可重复读	不可能	不可能	可能
    可串行化	不可能	不可能	不可能
    
SET TRANSACTION transaction_mode [, ...]
SET TRANSACTION SNAPSHOT snapshot_id
SET SESSION CHARACTERISTICS AS TRANSACTION transaction_mode [, ...]

 这里的 transaction_mode是下列之一:
    ISOLATION LEVEL { SERIALIZABLE | REPEATABLE READ | READ COMMITTED | READ UNCOMMITTED }
    READ WRITE | READ ONLY
    [ NOT ] DEFERRABLE
-- 如果两个并发事务试图同时修改帐号12345的余额，那我们很明显希望第二个事务是从已更新过的行版本上进行更新。
尽可能把事务声明为READ ONLY。
如果需要，可以使用连接池，控制活动连接数。这总是一个重要性能的考虑，但是在使用可串行化 事务的繁忙系统中尤其重要。
不要往单个事务里放入超过为了完整性目的而应该放入的东西。
不要让连接在"idle in transaction"中停留超过需要的时间。
消除显示锁，SELECT FOR UPDATE和 SELECT FOR SHARE不再需要，因为通过可串行化事务自动提供保护。
当系统因为谓词锁表的内存不足，强制合并多个页级别谓词锁到单个的关系级别谓词锁时， 可能导致可串行化失败率的增加。你可以通过增加max_pred_locks_per_transaction 来避免这个问题。
顺序扫描总是需要一个关系级别谓词锁。这可能会导致串行化失败率的增加。 减小random_page_cost 和/或增大cpu_tuple_cost可能有助于促使使用索引扫描。 一定要权衡事务回滚和重启的任何减少，和查询执行时间上的任何整体的变化。

BEGIN;  --开启事务
--事务隔离级别，定义多个事务时间的隔离级别
BEGIN TRANSACTION ISOLATION LEVEl [READ COMMITTED/REPEATABLE READ/SERIALIZABLE];  --一次启动事务并指定事务隔离级别
BEGIN;
TRANSACTION ISOLATION LEVEl [READ COMMITTED/REPEATABLE READ/SERIALIZABLE];  --先启动事务，再设置事务隔离级别
--预备事务，使得事务分阶段可以提交
PREPARE TRANSACTION 'foobar';
......
COMMIT PREPARE TRANSACTION 'foobar';
ROLLBACK PREPARE TRANSACTION 'foobar';
--保存点savepoint,可以支持事务的部分回滚
insert into lyy values(1,'nn');
savepoint svp1;
insert into lyy values(2,'ff');
rollback to savepoint svp1;
--此时提交的话，第二个insert未被插入，但是第一个插入成功。
END; 或者 COMMIT;  --结束并提交事务
或者 ROLLBACK;  --结束并回滚事务

```

## 明确封锁

```sql
-- 表级锁
-- 行级锁
-- 死锁
-- 咨询锁
```

## 性能提升技巧

```sql
-- 使用EXPLAIN 
EXPLAIN ANALYZE SELECT *
FROM tenk1 t1, tenk2 t2
WHERE t1.unique1 < 10 AND t1.unique2 = t2.unique2;

--  EXPLAIN ANALYZE
EXPLAIN ANALYZE SELECT *
FROM tenk1 t1, tenk2 t2
WHERE t1.unique1 < 10 AND t1.unique2 = t2.unique2;

-- 向数据库中添加记录
	--关闭自动提交  当使用多条INSERT时，关闭自动提交，并且只在结束的时候做一次提交。
	--使用COPY  使用COPY在一条命令里加载所有记录， 而不是一连串的INSERT命令。COPY命令是为加载数量巨大的数据行优化过的； 它没INSERT那么灵活，但是在大量加载数据的情况下，导致的开销也少很多。 因为COPY是单条命令，因此填充表的时候就没有必要关闭自动提交了。
	--删除索引 如果你正在加载一个新创建的表，最快的方法是创建表， 用COPY批量加载，然后创建表需要的任何索引。如果你对现有表增加大量的数据，可能先删除索引，加载表， 然后重新创建索引更快些。
	--删除外键约束 和索引一样，"批量地"检查外键约束比一行行检查更高效。因此，也许我们先删除外键约束， 加载数据，然后重建约束会更高效。
	--增大maintenance_work_mem 在加载大量的数据的时候，临时增大maintenance_work_mem配置变量可以改进性能。 这个参数也可以帮助加速 CREATE INDEX和ALTER TABLE ADD FOREIGN KEY命令
	--增大checkpoint_segments 临时增大checkpoint_segments配置变量也可以让大量数据加载得更快。
	--事后运行ANALYZE 不管什么时候，如果你在更新了表中的大量数据之后，运行ANALYZE都是个好习惯。 这包括批量加载大量数据到表。运行ANALYZE (或者VACUUM ANALYZE) 可以保证规划器有表数据的最新统计。
```

## 加密选项

```sql
PostgreSQL提供了几个不同级别的加密， 并且在保护数据库服务器不受数据窃贼、不道德管理员、不安全网络等因素泄漏的方面提供很高的灵活性。 加密可能也是保护一些诸如医疗记录和财务交易等敏感数据的要求。

口令存储加密
缺省的时候，数据库用户的口令以 MD5 散列的方式存储，所以管理员无法判断赋予用户的实际口令。 如果 MD5 加密用于客户端认证，那么未加密的口令甚至都不可能临时出现在服务器上， 因为客户端在透过网络发送之前，就先用 MD5 加密了。

为指定的字段加密
pgcrypto模块允许对某些字段进行加密存储。这个功能在某些数据是敏感的情况下有用。 客户端提供解密的密钥，然后数据在服务器端解密，然后发送给客户端。
在数据解密和数据在服务器与客户端之间传递时，解密数据和解密密钥将会在服务器端存在短暂的一段时间。 这就给那些可以完全访问数据库服务器的人提供了一个短暂的截获密钥和数据的时间， 这样的人一般是数据库管理员。

数据库分区加密
在 Linux 上，加密可以在使用"回环设备"(loopback device)挂载的文件系统上面进行。 这样就可以把磁盘上整个文件分区都加密，然后由操作系统解密。在 FreeBSD 上， 等效的设施叫 GEOM 基本磁盘加密(gbde)，并且许多其他操作系统支持这个功能，包括Windows。
这个机制避免了在整个计算机或者磁盘驱动器被窃的情况下，未加密的数据被从驱动器中读取。 它无法防止在文件系统被挂载的时候的攻击，因为在挂载之后，操作系统提供数据的解密视图。 不过，要想挂载文件系统，你需要有一些方法把解密密钥传递给操作系统， 有时候这个密钥就存储在挂载该磁盘的主机的某个地方。

跨网络加密口令
MD5认证方法在客户端将口令发给服务器之前双重加密之。第一次 MD5 加密是基于用户名的， 然后在连接数据库的时候，用服务器发送的随机盐粒再次加密。 这个双重加密的数值就是从网络传递给服务器的数值。双重加密不仅可以避免口令泄漏， 还可以避免稍后其它的连接使用同样的加密口令连接数据库(回放攻击)。

透过网络加密数据
SSL 连接加密所有透过网络发送的数据：口令、查询、返回的数据。pg_hba.conf 文件允许管理员声明哪些主机可以使用不加密的连接(host)， 以及哪些主机需要使用 SSL 加密的连接(hostssl)。客户也可以指定只通过SSL连接到服务器。 我们也可以使用Stunnel或SSH加密数据传输。

SSL 主机认证
客户端和主机都可以提供 SSL 证书给对方。这么做需要在两边都进行一些额外的配置工作， 但是这种方式提供了比简单使用用户名和口令更强的身份认证的手段。 它避免一个计算机装作是服务器，然后读取客户端口令， 只要时间长得足够读取客户端发送的口令就行了。它还避免了"中间人" 攻击(在客户端和服务器之间有台计算机，伪装成为服务器并且读取然后将所有数据在客户端和服务器之间传递)。

客户端加密
如果服务器机器的系统管理员是不可信的，那么客户端加密数据也是必要的；这种情况下， 未加密的数据从来不会在数据库服务器上出现。数据在发送给服务器之前加密， 而数据库结果必须在客户端使用之前解密。
```

