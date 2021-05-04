---
title: 数据-MySQL语言基础
date: 2020-08-10 09:24:52
categories:
	- Data Analysis
tags:
	- MySQL
	- DataBases
---

## 语言基础

注释：`#`

调用某个库：`use databases`

着重号`xxx`

<!--more-->

### DQL语言

1. 基础查询

   `select 查询列表 from 表名；`

- 查询列表：

  - 表中字段
  - 常量值
  - 表达式
  - 函数

- 查询结果是一个虚拟的表格

- 查询表中的单个字段

  `select last_name from  employees;`

- 查询表中的多个字段，用逗号隔开

  `select last_name,id from employees;`

  - **顺序随意 **- 可以自定义
  - 不知道怎么拼，直接在图形化界面点就可以
  - 按```F12```就能排列好顺序

- 查全部 - **顺序和表内会一模一样**

  ```mysql
  select * from employees
  ```

2. 查询常量值

   ```mysql
   select 100;
   
   select 'john';
   ```

   - 查询表达式

   ```mysql
   select 100*98;
   ```

   - 查询函数

   ```mysql
   select version();
   ```

3. 起别名 

   - 方式一：as

   - 在查询的字段有比较高的重复率时使用比较好

   ```mysql
   select 100%98 as 结果;
   
   select last_name as 姓,last_name as 名 from employees;
   ```

   - 方式二：空格

   ```mysql
   select last_name 姓,last_name 名 from employees;
   ```

   **注意别名不能使用特殊符号，如果使用了，请加双引号" " **

4. 去重 - distinct

   ```mysql
   select distinct id from mydatabase
   ```

5. +号的作用

   - +号在mysql中只有**运算符**的功能
   - 会试图将字符型转换为数值型，但是如果不能转换成功，**字符型数值转换为0**

6. 拼接

   - 比如把姓和名连接成一个字段：

     这样是不行的：

     ```mysql
     select last_name+first_name as 姓名 from employees;
     ```

   - **应当用concat 关键字进行连接**，括号里面使用的是着重符号，而不全是单引号，像逗号才使用单引号

     ```mysql
     select CONCAT('first_name','last_name') as 姓名;
     ```

     如果想用“，”隔开，那就使用：

     ```mysql
     select CONCAT('last_name',',','first_name') as out_put;
     ```

   - **如果其中有NULL字段，那么结果就会是NULL**

     为了消除这种null字段的影响，应当加上一些判断

     `IFNULL(id_a, '0')`函数可以判断`id_a`是不是null，如果是，就会取0；

7. 显示表的结构用 `DESC database;`

8. **条件查询**

   `select 查询列表 from 表名 where 筛选条件;`

   - 先执行from，然后执行筛选，最后是查询

   分类：

   - 按条件表达式筛选

     条件运算符：> < = ‘!=不等于’ ‘<>不等于’ <= >=

     关键是分清楚：要查什么，条件是什么

     > 查询：工资 > 12000 的员工信息
     >
     > ​	要查的是员工信息，条件是工资；
     >
     > 查询：部门编号不等于90的员工名和部门编号
     >
     > ​	要查的是员工名和部门编号，条件是部门编号不等于90；

   - 按逻辑运算符

     &&   ||    !  

     and  or  not

     **用于连接条件表达式**

     > 查询：工资在10000和20000之间的员工名，工资以及奖金
     >
     > 查询：部门编号不是在90和110之间，或者工资高于15000的员工信息

   - ==**模糊运算符**==

     **like**

     > 查询：员工名中包含字符a的员工信息
     >
     > ```mysql
     > select * 
     > from employees
     > where last_name LIKE '%a%';
     > ```

     - `%`表示一种通配符
     - 一般要和通配符结合使用：

     > % - 任意多个字符，包括 0 个字符
     >
     > _  - 任意单个字符
     >
     > **如果查询第二个字符为 _ 的员工，那么就要进行转义！**
     >
     > ```mysql
     > select last_name
     > from employees
     > where last_name like '_\_%';
     > 
     > # 也可以自定义一个转义符号，用ESCAPE字符
     > where last_name like '_$_%' escape '$';
     > ```

     **between and** - 提高简洁度

     > 查询员工编号在100和120之间的员工信息
     >
     > ```mysql
     > select *
     > from employees
     > where employee_id BETWEEN 100 AND 120;
     > 
     > # 明显好于之前的方法
     > where employee_id >= 100 AND employee_id <= 120;
     > ```

     - 区间包含临界值
     - between 后面的数字要比 and 后面的小，不能调换顺序，否则会出错！

     **in**

     - 判断某字段的值是否属于in列表中的某一值
     - in 列表中的值类型必须一致或者兼容（就是可以转换，例如 '123' 和 123）
     - 不能套用通配符

     > 查询：员工的工种编号 是 IT_PROG、AD_VP、AD_PRES中的员工名和工种编号
     >
     > ```mysql
     > select last_name, job_id
     > from employees
     > where job_id IN('IT_PROG', 'AD_VP', 'AD_PRES');
     > 
     > # 比使用OR来解决要方便很多
     > where job_id='IT_PROT' OR job_id='AD_VP' OR job_id='AD_PRES';
     > # 也就是说 in 和上面语句中的 等于号= 是一致的
     > ```

     **is null**

     - ==”普通的等于号=“== 不能判断NULL值，但是可以使用 “IS NULL” 进行判断
     - is null 或 is not null可以判断null值

     > 查询：没有奖金的员工名和奖金率
     >
     > ```mysql
     > select last_name, commission_pct
     > from employees
     > where commission_pct IS NULL;
     > ```
     >
     > 查询：有奖金的？==IS NOT NULL==

### DML语言

- 增删改

### DDL语言

### TCL语言、DCL语言