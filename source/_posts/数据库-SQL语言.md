---
title: 数据库-SQL语言
date: 2021-03-03 16:33:27
categories:
	- Data Analysis
tags:
	- MySQL
	- DataBases
---

怎么样建立起一个数据仓库的

- 肯定不是说把 csv 数据导入 mysql 这么简单，确实就像 布迩 说的，我这个算是别人建好的数据仓库
- 那么究竟怎样才是自己建立数据仓库呢？

csv 文件导入 MySQL：

```mysql
load data infile 'F:/MySqlData/test1.csv' -- CSV文件存放路径
into table student -- 要将数据导入的表名
fields terminated by '列的分隔符' optionally enclosed by '"' escaped by '"'
lines terminated by '\r\n' -- 回车换行符;
```

如果是linux下生成的文件，就是 `\n` 结尾

注意：这个东西很有可能会出现各种的报错，需要修改配置文件等内容

<!--more-->

> 参考：[将csv的数据导入mysql](https://www.cnblogs.com/yoyotl/p/9858587.html)

语句

```mysql
select * from student;
```

> 怎么样构建一个数据仓库
>
> 数据库索引的原则
>
> 数据库面试常见的问题
>
> 数据库面经找一找

# SQL

Structured Query Language 结构化查询语言

RDBMS 关系数据库管理系统

`use database;` 选择数据库

`set names utf8;` 设置使用的字符集

> SQL 对大小写不敏感
>
> 分号是分隔SQL语句的标准方法

`CREATE DATABASE` 创建新的数据库

`ALTER DATABASE` 修改数据库

`UPDATE` 更新数据库中的数据

`DELETE` 删除数据库中的数据

`INSERT INTO` 向数据库中插入新数据

---

`CREATE TABLE` 创建新表

`ALTER TABLE` 变更（改变）数据库表

`DROP TABLE` 删除表

---

`CREATE INDEX` 创建索引（搜索键）

`DROP INDEX` 删除索引

> 数据库 database 和 表 table 的区别
>
> SQL 和 MySQL 的关系
>
> SQL 有很多种，包括 SQL Sever/MS Access, MySQL, Oracle，SQL是语言，MySQL 是一种数据库软件

## 语句

### 创建

创建数据库

`CREATE DATABASE db_name`

创建数据表

```sql
CREATE TABLE table_name
(
ID int,
Name varchar(255),
Address varchar(255),
City varchar(255)
);
```

然后可以使用 `INSERT INTO` 向表中插入数据

可以对数据表内容进行约束，保证合规：

创建表时可以针对列进行内容的约束：

```sql
CREATE TABLE table_name
(
ID int constraint_name AUTO INCREMENT,
Name varchar(255) constraint_name,
Address varchar(255) constraint_name,
City varchar(255) DEFAULT ‘Sandnes’,
UNIQUE (ID),
PRIMARY KEY (Name),
CHECK (ID>0)
);
```

这里写的 `constraint_name` 可以是：

- NOT NULL  -  不能存储 NULL 值；再修改 为 “NULL”就可以存储 NULL 值了
- UNIQUE - 为列或者多个列提供唯一性的保证，一个表可以有多个，mysql需要单独在最后声明 `UNIQUE (P_Id)`
- PRIMARY KEY - 为列或者多个列提供唯一性的保证，但是每个表只能有一个
- FOREIGN KEY - 保护表直接的连接，可能有某个列是两个表之间的连接外键
- CHECK - 限制列中值的范围
- DEFAULT - 向列中插入默认值

如果表已经建立好了，可以使用

 `ALTER TABLE table_name MODIFY column_name type constrain_name;`

这些约束也可以删除，要使用 `ALTER TABLE table_name xxx constrain_name` 语句

其中xxx 位置：

- NOT NULL 用 MODIFY
- UNIQUE 用 DROP
- PRIMARY 用 DROP
- FOREIGN 用 DROP
- CHECK 用 DROP
- DEFAULT 用 DROP

`AUTO INCREMENT` 字段，每次插入新记录时，会自动地创建主键字段的值 

比如上面的例子中，ID就是这样的字段，默认开始值是1（也可以自己设置起始值），每条新纪录递增1

这样的话，向表中添加数据时，不需要为 ID 字段规定值（会自动生成）

### 索引

为了更快地查找数据，可以创建索引

> 但是索引是看不到的！

更新一个包含索引的表更花时间

一般只在最常搜索的列创建索引

`CREATE INDEX index_name ON table_name (column_name)` 创建简单索引，可以使用重复的值

`CREATE UNIQUE INDEX index_name ON table_name (column_name)` 创建唯一索引，不能使用重复的值（两个行不能索引值相同）

### 读取

`SELECT * FROM database;` 读取数据表的信息

- `*` 表示全部
- `*` 的位置可以被替换为一个或者多个列的 列名，多个列名之间用 `,` 进行区分
- 结果会存储在一个**结果表**中 —— **结果集**

上面的搜索中一个列可能会包含多个重复值，可以用

`SELECT DISTINCT * FROM table_name;` 就会只取出要选的那一列中不重复的值

要选取前多少个数据，使用到  `TOP` 语句，但是这个语句在不同的sql 语言种是不一样的，以 MySQL 为例，使用 `LIMIT` 进行限定：

```sql
SELECT column_name(s)
FROM table_name
LIMIT number;
```

可以选择满足条件的前 number 条记录



---

```sql
SELECT *
FROM table_name
WHERE column_name = value1
AND column_name > value2
```

如果 value 是数值，不用引号

如果 value 是文本值，要用引号

WHERE 子句中的运算符

- = 等于
- <> 不等于
- BETWEEN 在某个范围内 `BETWEEN value1 AND value2`
  - 介于 value1 和 value2 之间
  - 也可以添加 `NOT` 表示不在某个区间，比如 `NOT BETWEEN xxx`
  - value1 和 value2 可以都是数字，**也可以是字母**，还可以是日期date（`2016-05-10` 之类的）
  - MySQL中，BETWEEN 区间 是**包括两个边界的值**的！ NOT BETWEEN 就不包括两个边界的值了
- LIKE 搜索某种模式 `LIKE pattern` 
  - 这个指定模式是用通配符来进行设定的
  - 比如 `G%`  表示以 G 开头的字符值
  - 还可以使用  `NOT LIKE pattern` 排除某个模式
- IN 指定针对某个列的多个可能值
  - 使用在指向比较明确的场合
  - 比如 `WHERE column_name IN (value1, value2, ...)`
  - 那么该 column 的值一定要在指定的 value 中
  - IN 和 = 的区别是，IN 可以规定多个值，= 只能规定一个值，可以互相转换，使用 `OR`

> 通配符
>
> - `%` 替代0-n个字符
> - `-` 替代 1 个字符
>
> 正则表达式
>
> `REGEXP 'pattern'`和 `NOT REGEXP`
>
> （或者`RLIKE`和 `NOTRLIKE`）
>
> 例如
>
> ```sql
> SELECT * FROM table_name
> WHERE column_name REGEXP '^[A-H]'
> ```
>
> 表示的是以A到H 开头的字符
>
> `REGEXP '^[^A-H]'` 表示的是，不以A到H开头的字符

子句

`AND`  两个条件都成立，才会显示一条记录

`OR` 两个条件有一个成立，就会显示一条记录

这两个符号可以结合起来，还可以加括号，这样构成复杂语句

```SQL
SELECT column_name
FROM table_name
ORDER BY column_name ASC|DESC;
```

`ORDER BY` 对结果集按照某个列（或者某几个列）进行排序

`DESC` 是按照降序排序

> 如果按照多个列进行排序
>
> 先按照第一列，然后按照第二个列，两个列都会考虑
>
> 比如
>
> ![image-20210302223234109](https://typoraim.oss-cn-shanghai.aliyuncs.com/image/image-20210302223234109.png)

### 增删改

```sql
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3,...);
```

`INSERT INTO`

可以用来插入一个新行的数据

也可以指定某几个列插入数值，没有说到的列就是0

> 列表的第一列是“id”
>
> “id”会自动更新

【MySQL不支持】`SELECT INSERT INTO`用来从一个表中复制数据（行），然后把数据插入到另一个新表【这个表原本不存在】中

【MySQL 支持】`INSERT INTO ... SELECT` 也是用来从一个表中复制数据（行），然后插入到一个已存在的表中【这个被插入的表中已经存在的行不会受影响】

```sql
INSERT INTO table2
(column_names)
SELECT column_names
FROM table1;
```

`ALTER TABLE` 添加、删除、修改列

`ALTER TABLE table_name ADD column_name datatype`  添加列

删除，alter … drop

修改表中数据的类型  alter … modify 

```sql
UPDATE table_name
SET column1=value1, column2=value2, ...
WHERE some_column=some_value;
```

`UPDATE` 语句可以用来更新表中的记录

注意一定要使用 `WHERE` 语句来限定需要更新的记录，要不然，所有记录都会被更新

```SQL
DELETE FROM table_name
WHERE some_column=some_value;
```

`DELETE` 语句用来删除表中的记录（行）

注意要使用 `WHERE` 语句限定需要删除的记录，否则所有记录会被删除

> 删库跑路
>
> `DELETE * FROM table_name;`
>
> 或者 `DELETE FROM table_name;` 会删除全部行

`DROP` 可以用来删除 索引、表和数据库

`ALTER TABLE table_name DROP INDEX index_name`  删除表中的索引

`DROP TABLE table_name` 删除表

`DROP DATABASE db_name` 删除数据库

`TRUNCATE TABLE table_name` 删除表内数据，但不删除表本身

### 起别名 AS

```sql
SELECT column_name AS alias_name FROM table_name;  -- 给列起别名
SELECT column_name FROM table_name AS alias_name;  -- 给表起别名
```

多个列需要分别 AS

`CONCAT (column1, column2) AS column3` 可以将多个列结合在一起，创建一个 column3 的别名

```sql
SELECT name, CONCAT(url, ',', alexa, ',', conntry) AS site_info FROM table_name;
```

起别名对于在多个表中进行混合查询比较有利，比如

```sql
SELECT w.name, w.url, a.count, a.data
FROM Websites AS w, access_log AS a
WHERE a.site_id=w.id and w.name="菜鸟教程"
```

总结来说，别名的好处就是：

- 查询多个表
- 查询中使用了函数
- 列名称很长
- 多列结合

比较方便

> 如果在多个表中进行查询，不同表中的列名需要添加各自的前缀

### 多表合并

SQL join 用于把来自两个或者多个表的行结合起来

主要分四类

- LEFT JOIN - 即使右表没有匹配，也会从左表返回行
- RIGHT JOIN - 即使左表没有匹配，也会从右表返回行
- INNER JOIN - 在表中存在至少一个匹配时返回行【比如表A的行，在表B中没有匹配，那么这些行就不会列出】
- FULL JOIN - 只要一个表存在匹配，就返回行 ==但是MySQL中不支持 FULL OUTER JOIN==

但是具体还有方法，总共有7种相关的用法

最常见：`INNER JOIN` 从多个表中返回满足JOIN条件的所有行

```sql
SELECT w.id, w.name, a.count, a.date
FROM Websites as w
INNER JOIN access_log as a
ON w.id=a.site_id;
```

ON  后面会跟随连接的条件【也就是说，两个表是通过这个参数列连起来的】

最后返回的结果集中，将只包含所搜寻的四个参数：w.id, w.name, a.count, a.date

```sql
SELECT w.name, a.count, a.date
FROM Websites as w
LEFT JOIN access_log as a
ON w.id=a.site_id
ORDERED BY a.count DESC;
```

即使 表 a（access） 中没有与 w.id 相同的 site_id， 那么 w.id 所在的数据还是会存在于结果中

这个时候，这一行里面没有的元素就会用 `NULL` 来填充

`RIGHT JOIN` 是相似的

`UNION` 操作符：合并多个 SELECT 语句的结果集

有两种用法：

第一种是 `UNION`，那么不会保留重复值

```sql
SELECT country FROM Websites
UNION
SELECT country FROM apps
ORDER BY country;
```

另一种是 `UNION ALL`，会保留重复值

还可以在加上 `WHERE` 等语句 再进行筛选

> `WHERE` 语句一般加在 `ORDER BY` 前面

### 视图（VIEWS）

视图 - 可视化的表

感觉很像是从表中选择了部分数据，不是特别具体

似乎 Northwind 样本数据库使用的比较多

### MySQL 中的时间 Dates

有一些内建时间函数，可以参考：https://runoob.com/sql/sql-dates.html

比如 NOW() 返回当前的时间和日期 - 分为两个部分 CURDATE() 是当前的日期，CURTIME() 是当前的时间

可以使用 `DATA_FORMATE(date, format)` 函数来指定 想要的显示 日期/时间数据的格式，format 部分就是正则式

## 数据类型

NULL - 遗漏的未知数值，不是0，不能和0比较

要选用 为 NULL 的数值，应该用 `WHERE column_name IS NULL`

不选用，则是 `IS NOT NULL`

MySQL 中还有 `IFNULL(column_name, 0)`函数，用来处理，如果这个值是 NULL，那么就会转化为0

或者 `COALESCE(column_name, 0)` 函数，如果是NULL，转为0

SQL中常用的数据类型，以Mysql为例子：

- int
- float
- Char - 也就是 string，最多255，固定长度
- Varchar - 也就是string（variable）一类的， 可变长度，最多255
- blob或者 text - 也就是 binary object

每种具体的数据库里面可能是不太一样的

Mysql中主要三类

- Text 类型，也分 tiny、medium、long的
- number 类型，分 tiny、small、medium、big、float、double等
- date 类型，分日期的、日期时间的、时间戳（timestamp）、年的

### 注释


注释方法：

- `–` 单行注释，两个 - ，加一个空格
- `#` 单行注释
- `/* */` 多行注释

> 自检查：[SQL数据库面试题以及答案（50例题）](https://blog.csdn.net/hundan_520520/article/details/54881208?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control&dist_request_id=1328593.8838.16147378952884227&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control)

## 常用原则

> 参考：[数据库常见面试题（附答案）](https://blog.csdn.net/qq_22222499/article/details/79060495)

基本表的性质【事务四大特征】

- 原子性：要么执行，要么不执行
- 隔离性：所有操作全部执行完以前，其他会话不能看到过程
- 一致性：事务前后，数据总额一致
- 持久性：一旦事务提交，对数据的改变是永久的

索引分类（5种），索引失效条件

- 普通索引：最基本的索引，没有任何限制
- 唯一索引：索引列的值必须唯一，可以空值
- 主键索引：唯一索引，不允许空值
- 全文索引：耗时耗空间
- 组合索引：提高效率，遵循“最左前缀”原则

关于数据库有几种索引

- 按索引列的唯一性，可以分为唯一索引和非唯一索引
- 按索引列的个数：单列索引和符合索引
- 按索引列的物理组织方式：B树索引、位图索引、反向键索引、函数索引、删除索引、重建索引
