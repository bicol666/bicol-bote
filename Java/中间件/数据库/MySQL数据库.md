# MySQL数据库笔记

# 第一章：MySQL数据库常用命令

> **提醒：**本章节命令独属于MySQL数据库，不具备通用性，其他数据库不适用！！！！！！！！！

## 1、 登录MySql数据库(`root`账户)

- **语法格式**

  方式一、显示的登录，暴露密码，不安全。

  ```sql
  mysql -h 主机IP/域名 -P 端口号 -u root -p 账户的密码
  ```

  默认主机IP`127.0.0.1`以及默认端口号`3306`均可省略；

  

  方式二、隐藏密码登录。

  ```sql
  mysql -h 主机IP/域名 -u root -p
  ```

  按`Enter`键之后直接输入密码，此时输入的密码不会在窗口有任何显示（安全），密码输入完毕后再按`Enter`即可完成登录；

  

- **示例**

```sql
C:\Users\null'pointer>mysql -h127.0.0.1 -uroot -p123456
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 5
Server version: 5.7.14-log MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

见到上面的`欢迎界面`就说明登录成功了，欢迎界面主要描述了如下的信息：

-  每行sql命令以`;`或`\g`结束；
- `Your MySQL connection id is 5`：你的MySQL连接次数是5，也就是说你到目前为止连接了MySQL5次，每连接一次，记录就会增加一次；
- `Server version: 5.7.14-log MySQL Community Server (GPL)`：你的MySQL版本是社区版5.7.14；
- 接下来是版权所有：Oracle，MySQ现在是Oracle公司的产品；
- 通过命令`help`或者`\h`来查看关于MySQL数据库的命令；
- 通过命令`\c`来结束当前输入的的操作(命令)；



- **注意事项**

  - 1、若登录的是本机的MySql数据库可省略主机IP参数（-h……）；

    ```sql
    mysql -uroot -p密码
    ```

  - 2、若不想让密码明文显示在doc命令窗口上可在输入`-p`之后回车再输入密码，此时密码就会以`*`的形式显示在屏幕上,达到保密的效果；

    ```sql
    mysql -uroot -p
    ```

    

  

## 2、显示已有的数据库

- **语法**

```sql
show databases;
```

## 3、使用数据库

- **语法**

```sql
use 数据库名;
```

- **示例**

```sql
mysql> use test;
Database changed
```



## 4、查看当前使用的数据库

- **语法**

```sql
select database();
```

- **示例**

```sql
mysql> select database();
+------------+
| database() |
+------------+
| test       |
+------------+
1 row in set (0.00 sec)
```



## 5、查看当前数据库中有哪些表

- **语法**

```sql
show tables;
```

- **示例**

```sql
mysql> show tables;
+----------------+
| Tables_in_test |
+----------------+
| dept           |
| emp            |
| salgrade       |
+----------------+
3 rows in set (0.00 sec)
```



## 6. 显示其他数据库中有哪些表

- **语法**

  ```sql
  show tables from 数据库名;
  ```

- **示例**

  ```sql
  mysql> show tables from world;
  +-----------------+
  | Tables_in_world |
  +-----------------+
  | city            |
  | country         |
  | countrylanguage |
  +-----------------+
  ```



## 7、显示表结构

- **语法**

  方式一

  ```sql
  desc 表名;
  ```

  方式二

  ```sql
  describe 表名;
  ```

- **示例**

```sql
mysql> desc emp;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| EMPNO    | int(4)      | NO   | PRI | NULL    |       |
| ENAME    | varchar(10) | YES  |     | NULL    |       |
| JOB      | varchar(9)  | YES  |     | NULL    |       |
| MGR      | int(4)      | YES  |     | NULL    |       |
| HIREDATE | date        | YES  |     | NULL    |       |
| SAL      | double(7,2) | YES  |     | NULL    |       |
| COMM     | double(7,2) | YES  |     | NULL    |       |
| DEPTNO   | int(2)      | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
```



## 8、显示建表语句

- **语法**
```sql
show create table 表名;
```

- **示例**

```sql
mysql> show create table emp;
+-------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table | Create Table                                                                                                                                                                                                                                                                                                                                 |
+-------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| emp   | CREATE TABLE `emp` (
  `EMPNO` int(4) NOT NULL,
  `ENAME` varchar(10) DEFAULT NULL,
  `JOB` varchar(9) DEFAULT NULL,
  `MGR` int(4) DEFAULT NULL,
  `HIREDATE` date DEFAULT NULL,
  `SAL` double(7,2) DEFAULT NULL,
  `COMM` double(7,2) DEFAULT NULL,
  `DEPTNO` int(2) DEFAULT NULL,
  PRIMARY KEY (`EMPNO`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
+-------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

可以发现结果以表格的形式输出，由于窗口太小，显示得非常混乱，可以加上参数`\G`来美化一下：

```sql
mysql> show create table emp \G
*************************** 1. row ***************************
       Table: emp
Create Table: CREATE TABLE `emp` (
  `EMPNO` int NOT NULL,
  `ENAME` varchar(10) DEFAULT NULL,
  `JOB` varchar(9) DEFAULT NULL,
  `MGR` int DEFAULT NULL,
  `HIREDATE` date DEFAULT NULL,
  `SAL` double(7,2) DEFAULT NULL,
  `COMM` double(7,2) DEFAULT NULL,
  `DEPTNO` int DEFAULT NULL,
  PRIMARY KEY (`EMPNO`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
1 row in set (0.00 sec)
```

注意加上`\G`之后结尾**没有分号**，加上分为会报错；

## 9、查看MySQL的版本号

- **语法**

  方式一、未登录在命令行窗口：

  ```shell
  mysql --version
  ```

  方式二、登录之后：

  ```sql
  select version();
  ```

## 10、 结束（取消执行）一条语句

- **语法**

  方式一：
  
  ```sql
  \c
  ```
  
  方式二：
  
  ```sql
  clear
  ```
  
  

## 11、退出登录MySql数据库

- **语法**

  方式一：
  
  ```sql
  exit
  ```
  
  方式二：
  
  ```sql
  quit
  ```
  
  

## 12、创建数据库

- **语法**

  ```sql
  create database 数据库名;
  ```



## 13、执行SQL脚本文件（导入数据）

> 1、`sql`脚本文件当中的数据量太大的话可以使用`source`命令来初始化数据库；
>
> 2、在执行该命令之前需要创建并选中数据库（use database）；

- **语法**

  ```sql
  source  sql脚本文件路径;
  ```


## 14、强制退出命令

> 当前SQL语句出现错误而又不想它执行时，组合键`ctrl+C`可以强制退出当前操作；





## 15、修改密码（8.0.x）

### 在知道密码的情况下

登录mysql

```mysql
mysql -u root -p
```

选中`mysql`数据库

```mysql
use mysql;
```

修改密码

```mysql
  ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '你的新密码';  
```





# 第二章：SQL语言的规则与规范

## 1、基本规则

- SQL可以写在一行或者多行。为了提高可读性，各子句分行写，必要时使用缩进。

- 每条命令以`；`或`\g`或`\G`结束。

- 关键字不能被缩写也不能分行。

- 关于标点符号

  - 必须使用英文状态下的半角输入方式。

  - `字符串型`和`日期时间类型`的数据可以使用单引号表示。列的别名，尽量使用双引号`""`表示，而且不

    议省略`as`。

## 2、SQL大小写规范

- MySQL在`Windows`环境下不区分小写，Windows操作环境本身也不区分大小写。
- MySQL在`Linux`环境下`默认`严格区分大小写，但是关键字、函数名、列名及其别名不区分大小写。



## 3、注释

- 单行注释

  ```sql
  # 这是一行注释
  ```

- 单行注释

  ```sql
  -- 这是一行注释，`--`后面必须带有一个空格；
  ```

- 多行注释

  ```sql
  /*  z  */
  ```

  



# 第三章：DQL（数据查询语言）

> DQL（Data Query language）数据查询语言，主要用于从数据库表里查数据，使用`SELECT`命令；
>
> 在所有的SQL语句当中`SELECT`查询语句最复杂，最重要；

## 1、 简单查询

- **语法**

  ```sql
  select 字段1，字段2，字段3，…… from 表名;
  ```

  字段名可以填写通配符【`*`】表示查询所有字段，在实际开发当中不建议使用，因为效率较低;

  

- **示例**

  ```sql
  mysql> select ename,sal,job from emp;
  +--------+---------+-----------+
  | ename  | sal     | job       |
  +--------+---------+-----------+
  | SMITH  |  800.00 | CLERK     |
  | ALLEN  | 1600.00 | SALESMAN  |
  | WARD   | 1250.00 | SALESMAN  |
  | JONES  | 2975.00 | MANAGER   |
  | MARTIN | 1250.00 | SALESMAN  |
  | BLAKE  | 2850.00 | MANAGER   |
  | CLARK  | 2450.00 | MANAGER   |
  | SCOTT  | 3000.00 | ANALYST   |
  | KING   | 5000.00 | PRESIDENT |
  | TURNER | 1500.00 | SALESMAN  |
  | ADAMS  | 1100.00 | CLERK     |
  | JAMES  |  950.00 | CLERK     |
  | FORD   | 3000.00 | ANALYST   |
  | MILLER | 1300.00 | CLERK     |
  +--------+---------+-----------+
  15 rows in set (0.00 sec)
  ```



## 2、字段别名

> 我们可以对查询出来的字段（列）起一个别名，而不使用原来的列名。

- **语法**

  方式一：使用关键字`AS`。

  ```sql
  select 字段 as 字段的别名 from 表名; 
  ```

  方式二：不适用关键字。

  ```sql
  select 字段 字段的别名 from 表名;
  ```

  方式三：加双引号，不建议使用单引号，因为不符合规范。双引号内可以写空格。

  ```sql
  select 字段 "字段的别名" from 表名
  ```

  

- **示例**

```sql
mysql> select ename as 'name',sal as "薪资" from emp;
+--------+---------+
| name   | 薪资    |
+--------+---------+
| SMITH  |  800.00 |
| ALLEN  | 1600.00 |
| WARD   | 1250.00 |
| JONES  | 2975.00 |
| MARTIN | 1250.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| SCOTT  | 3000.00 |
| KING   | 5000.00 |
| TURNER | 1500.00 |
| ADAMS  | 1100.00 |
| JAMES  |  950.00 |
| FORD   | 3000.00 |
| MILLER | 1300.00 |
+--------+---------+
15 rows in set (0.00 sec)
```

- **注意**
  - 1、重命名的字段如果是`中文`需要用`单引号`或者`双引号`括起来，表示这是一个字符串。

  - 2、标准的sql语句中`字符串`必须使用`单引号`括起来；
  - 3、起别名的时候`as`关键字可以省略；



## 3、去除重复行

> 默认情况下查询是会返回所有行记录，包括重复行。想要去除重复行需要使用`distinct`关键字。

- **语法**

  ```sql
  select distinct 要去重的列 from 表名;
  ```

- **注意**：
  - 1、`distinct`必须放在所有列的最前面，否则报错。
  - 2、`distinct`后面可以跟多个列，这表示后面多个列联合去重。



## 4、着重号

> 在设计数据库的时候，我们应该尽量避免表名、列名等与数据库关键字（保留字）相同。但是如果真的发生了冲突可以使用着重号【反撇：``】来解决问题。

- **示例**

  ```sql
  假设有一张表的名字叫做`order`，我们要查询所有的记录。
  错误的SQL：
  SELECT * FROM ORDER;
  正确的SQL：
  SELECT * FROM `ORDER`;
  ```



## 5、查询常量列

> 通常发生于我们想要往表中插入一个字段，这个字段的值为一个固定不变的常量，这时我们直接在select语句字段那里写这个常量即可。
>
> 这个字段只是在查询时临时添加到结果集里的，并不存在于表中。
>
> `总结`：查询一个常量列，输出结果就是这个常量；

- **示例**

  ```sql
  
  mysql> select '这是新增的列' 新增的列,ename from emp;
  +--------------+--------+
  | 新增的列     | ename  |
  +--------------+--------+
  | 这是新增的列 | SMITH  |
  | 这是新增的列 | ALLEN  |
  | 这是新增的列 | WARD   |
  | 这是新增的列 | JONES  |
  | 这是新增的列 | MARTIN |
  | 这是新增的列 | BLAKE  |
  | 这是新增的列 | CLARK  |
  | 这是新增的列 | SCOTT  |
  | 这是新增的列 | KING   |
  | 这是新增的列 | TURNER |
  | 这是新增的列 | ADAMS  |
  | 这是新增的列 | JAMES  |
  | 这是新增的列 | FORD   |
  | 这是新增的列 | MILLER |
  +--------------+--------+
  14 rows in set (0.00 sec)
  ```

  

## 6、WHERE子句过滤

> 对于查询出来的数据，我们可以使用`where`子句进行过滤筛选，where必须放在`from`关键字的后面；


- **语法**

```sql
  select 字段1,字段2,…… from 表名 where 过滤条件;
```


## 7、运算符

> where、having等关键字会把运算结果不为1（真）的记录过滤掉。

### 7.1、算术运算符

> 算术运算符主要用于数学运算，其可以连接运算符前后的两个数值或表达式，对数值或表达式进行加 （+）、减（-）、乘（*）、除（/）和取模（%）运算。
>
> 算术运算符只要有`NULL`参与，结果就为NULL。

| 运算符 | 作用                                   |
| :----: | :------------------------------------- |
|   +    | 计算两个值或表达式的和。               |
|   -    | 计算两个值或表达式的差。               |
|   *    | 计算两个值或表达式的乘积。             |
| /或DIV | 计算两个值或表达式的商。               |
| %或MOD | 计算两个值或表达式的余数（取模运算）。 |



### 7.2、比较运算符

> 比较运算符用于对运算符左右两边的对象（值、表达式、字符串等）进行比较，比较结果为真返回1，比较结果为假返回0，其他情况（有NULL参与比较）返回`NULL`。
>
> 比较运算符一般用于`SELECT`语句的查询条件。
>

- **比较运算符**


|     运算符      | 作用                                   |
| :-------------: | -------------------------------------- |
|        =        | 判断两个值、表达式、字符串是否相等。   |
|       <=>       | 安全的判断两个对象是否相等。           |
|      <>&!=      | 判断两个对象是否不相等。               |
|        <        | 判断前面的对象是否小于后面的对象。     |
|       <=        | 判断前面的对象是否小于等于后面的对象。 |
|        >        | 判断前面的对象是否大于后面的对象。     |
|       >=        | 判断前面的对象是否大于等于后面的对象。 |
| IS NULL、ISNULL | 判断一个值是否为空（NULL）。           |
|   IS NOT NULL   | 判断一个值是否不为空（NULL）。         |
|   BETWEEN AND   | 判断一个值是否在一个区间内（闭区间）。 |

- 运算符：`=`

=用来比较运算符两边的运算对象是否相等，相等则返回真（1），否则返回假（0）。

具体的比较规则如下：

1、两边的运算对象如果存在NULL，返回NULL。

2、一边为数值一边为字符串，先将字符串转换为数值再做比较。

注意：运算符`=`不能与NULL值判断。



- 运算符：`<=>`

<=> 操作符和 = 操作符类似，不过 <=> 可以用来判断 NULL 值。



- 运算符：`IS NULL`&`ISNULL`

IS NULL 或 ISNULL 用来检测一个值是否为 NULL，如果为 NULL，返回值为 1，否则返回值为 0。ISNULL 可以认为是 IS NULL 的简写，去掉了一个空格而已，两者的作用和用法都是完全相同。

```sql
mysql> select 100 IS NULL from dual;
+-------------+
| 100 IS NULL |
+-------------+
|           0 |
+-------------+
1 row in set (0.00 sec)
```



- 运算符：`BETWEEN AND`

BETWEEN AND 运算符用来判断某个值是否位于两个数之间，或者说是否位于某个范围内，这个范围是`闭区间`。

注意：`AND`左边的数值必须小于右边的数值。





- **示例**


```sql
#示例1：查询工资等于5000的员工；
mysql> select ename,job from emp where sal = 5000;

#示例2：查询SMITH的职位和工资；
mysql> select job,sal from emp where ename = 'SMITH';

#示例3：查询工资大于等于3000的员工；
mysql> select ename,job,sal from emp where sal >= 3000;
```



### 7.3、逻辑运算符

> 逻辑运算符主要用来判断表达式的真假，在MySQL中，逻辑运算符的返回结果为1、0或者NULL。

- **逻辑运算符**

| 运算符       | 作用       |
| ------------ | ---------- |
| NOT  或  ！  | 逻辑：非   |
| AND  或  &&  | 逻辑：与   |
| OR  或  \|\| | 逻辑：或   |
| XOR          | 逻辑：异或 |

- **逻辑运算符：非**

将真（1）取反为假（0）。

```sql
mysql> select !0 from dual;
+----+
| !0 |
+----+
|  1 |
+----+
1 row in set, 1 warning (0.04 sec)
```



- **逻辑运算符：与**

`与运算符`两边的运算对象都必须是真（1），运算结果才返回真（1），其它情况均返回（0）。

```sql
#左右两边均为真（1）
mysql> select 1>0 && 0=0 from dual;
+------------+
| 1>0 && 0=0 |
+------------+
|          1 |
+------------+

#有一边为假（0）
mysql> select 1>0 && 0 from dual;
+----------+
| 1>0 && 0 |
+----------+
|        0 |
+----------+

#示例：找出工资在1000到3000之间的员工；
mysql> select ename,sal from emp where sal >=1000 and sal <=3000;
+--------+---------+
| ename  | sal     |
+--------+---------+
| ALLEN  | 1600.00 |
| WARD   | 1250.00 |
| JONES  | 2975.00 |
| MARTIN | 1250.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| SCOTT  | 3000.00 |
| TURNER | 1500.00 |
| ADAMS  | 1100.00 |
| FORD   | 3000.00 |
| MILLER | 1300.00 |
+--------+---------+
12 rows in set (0.00 sec)
```



- **逻辑运算符：或**

`或运算符`两边的运算对象只要有一边是真（1），运算结果就是真（1），其余情况均为假（0）。

```sql
#有一边为假（0）
mysql> select 0 || 1 from dual;
+--------+
| 0 || 1 |
+--------+
|      1 |
+--------+
1 row in set, 1 warning (0.00 sec)

# 左右两边均为假（0）
mysql> select 0|| 0 from dual;
+-------+
| 0|| 0 |
+-------+
|     0 |
+-------+
1 row in set, 1 warning (0.00 sec)
```



- **逻辑运算符：异或**

`异或运算符`只要两边的对象是一真（1）一假（0），返回结果就是真（1），其余均返回假（0）。

```sql
#左右两边均为真（1）
mysql> select 1 xor 1 from dual;
+---------+
| 1 xor 1 |
+---------+
|       0 |
+---------+
1 row in set (0.00 sec)

#一真（1）一假（0）
mysql> select 0 xor 1 from dual;
+---------+
| 0 xor 1 |
+---------+
|       1 |
+---------+
1 row in set (0.00 sec)
```



### 7.4、IN和NOT IN

运算符`in`等同于`or`是`在这几个值当中的意思`，在执行效率上和`or`没有区别；

```sql
#示例：找出工作岗位是MANAGER或SALESMAN的员工；
#方式1
mysql> select ename,job from emp where job = 'salesman' or job = 'manager';
+--------+----------+
| ename  | job      |
+--------+----------+
| ALLEN  | SALESMAN |
| WARD   | SALESMAN |
| JONES  | MANAGER  |
| MARTIN | SALESMAN |
| BLAKE  | MANAGER  |
| CLARK  | MANAGER  |
| TURNER | SALESMAN |
+--------+----------+
7 rows in set (0.00 sec)
#方式2
mysql> select ename,job from emp where job in ('salesman','manager');
+--------+----------+
| ename  | job      |
+--------+----------+
| ALLEN  | SALESMAN |
| WARD   | SALESMAN |
| JONES  | MANAGER  |
| MARTIN | SALESMAN |
| BLAKE  | MANAGER  |
| CLARK  | MANAGER  |
| TURNER | SALESMAN |
+--------+----------+
7 rows in set (0.00 sec)
#可以发现二者查询的结果一模一样
```

注意：`字段 in (1000,2000)`并不等同于`字段 >= 1000 and 字段 <= 2000 `，而是等同于`字段 = 1000 or 字段 = 2000`；**括号里的值不是代表一个区间，注意区分**；



`not in`：不在这几个值当中；

```sql
#示例：找出工作岗位不是MANAGER或SALESMAN的员工；
mysql> select ename,job from emp where job not in ('manager','salesman');
+--------+-----------+
| ename  | job       |
+--------+-----------+
| SMITH  | CLERK     |
| SCOTT  | ANALYST   |
| KING   | PRESIDENT |
| ADAMS  | CLERK     |
| JAMES  | CLERK     |
| FORD   | ANALYST   |
| MILLER | CLERK     |
+--------+-----------+
8 rows in set (0.00 sec)
```



### 7.5、模糊查询：LIKE

- `like`用于模糊查询，比如需求`找出姓李的员工`就需要使用到`like`关键字；
- 模糊查询时`_`代表一个字符，`%`代表多个字符；
- 如有特殊需求，可以在`_` 、`%`前面加反斜杠对其进行转义；



- **示例**

```sql
#示例：找出名字中第二个字母是'a'的员工；
mysql> select ename from emp where ename like '_a%';
+--------+
| ename  |
+--------+
| WARD   |
| MARTIN |
| JAMES  |
+--------+
3 rows in set (0.01 sec)
```



### 7.6、运算符的优先级

| 优先级 | 运算符                                                       |
| :----: | ------------------------------------------------------------ |
|   1    | =(赋值运算）、:=                                             |
|   2    | \|\|、OR                                                     |
|   3    | XOR                                                          |
|   4    | &&、AND                                                      |
|   5    | NOT                                                          |
|   6    | BETWEEN、CASE、WHEN、THEN、ELSE                              |
|   7    | =(比较运算）、<=>、>=、>、<=、<、<>、!=、 IS、LIKE、REGEXP、IN |
|   8    | \|                                                           |
|   9    | &                                                            |
|   10   | <<、>>                                                       |
|   11   | -(减号）、+                                                  |
|   12   | *、/、DIV、MOD                                               |
|   13   | ^                                                            |
|   14   | -(负号）、~（按位取反）                                      |
|   15   | ！                                                           |
|   16   | ()                                                           |

优先级数值越大，优先级越高。由此可见，小括号`()`的优先级是最高的。

一般情况下，级别高的运算符优先进行计算，如果级别相同，MySQL 按表达式的顺序从左到右依次计算。

## 8、分页【LIMIT】

> 1、不通用，limit是**MySQL中特有的**，Oracle中有一个类似的机制：rownum；
>
> 2、作用：选取结果集中的**部分数据**；
>
> 3、limit在`SELECT`语句中是最后执行的；

- **语法**

  ```sql
  # 方式一：
  limit	offset,	length;
  # 方式二：这约等于【limit	0,length】
  limit	length;
  ```
  
  - 方式二约等于：limit	0,length。
  - 参数说明：
    - 1、`offset`：指记录开始的偏移量，从`0`开始。
    - 2、`length`：长度，表示从第`offset + 1`条记录开始，取`length`条记录。
    - 3、参数值必须是`整数常量`。
    - 4、为了与 PostgreSQL 兼容，MySQL 也支持句法： LIMIT # OFFSET #。
  



## 9、子查询【嵌套查询】







## 10、排序【ORDER BY】

当数据被我们从数据库查出来后我们可以再对其进行排序，排序使用的是`order by`命令；

**升序排列**

示例

```sql
#示例：展出员工的薪资和名字，并且按照员工的工资升序排序；
#方式1：使用`order by`关键字，默认是升序排列；
mysql> select ename,sal from emp order by sal;
#方式2：添加`asc`关键字，asc代表升序排列；
mysql> select ename,sal from emp order by sal asc;

#结果
+--------+---------+
| ename  | sal     |
+--------+---------+
| SMITH  |  800.00 |
| JAMES  |  950.00 |
| ADAMS  | 1100.00 |
| WARD   | 1250.00 |
| MARTIN | 1250.00 |
| MILLER | 1300.00 |
| TURNER | 1500.00 |
| ALLEN  | 1600.00 |
| CLARK  | 2450.00 |
| BLAKE  | 2850.00 |
| JONES  | 2975.00 |
| SCOTT  | 3000.00 |
| FORD   | 3000.00 |
| KING   | 5000.00 |
+--------+---------+
```



**降序排列**

示例

```sql
#使用`desc`关键字指定降序排列；
#示例：找出员工名字和其工资，按照工资的降序排列；
mysql> select ename,sal from emp order by sal desc;
+--------+---------+
| ename  | sal     |
+--------+---------+
| SCOTT  | 3000.00 |
| FORD   | 3000.00 |
| JONES  | 2975.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| ALLEN  | 1600.00 |
| TURNER | 1500.00 |
| MILLER | 1300.00 |
| WARD   | 1250.00 |
| MARTIN | 1250.00 |
| ADAMS  | 1100.00 |
| JAMES  |  950.00 |
| SMITH  |  800.00 |
+--------+---------+
```



### 1）多字段排序

---

> 多个字段参与排序，`order by`关键字后越靠前的字段优先级越高；

示例

```sql
#多个字段参与排序，`order by`关键字后越靠前的字段优先级越高；
#示例：找出员工的名字和工资，按照员工的工资降序排列，工资相同的员工按照名字升序排列；
mysql> select ename,sal from emp order by sal desc,ename asc;
+--------+---------+
| ename  | sal     |
+--------+---------+
| KING   | 5000.00 |
| FORD   | 3000.00 |
| SCOTT  | 3000.00 |
| JONES  | 2975.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| ALLEN  | 1600.00 |
| TURNER | 1500.00 |
| MILLER | 1300.00 |
| MARTIN | 1250.00 |
| WARD   | 1250.00 |
| ADAMS  | 1100.00 |
| JAMES  |  950.00 |
| SMITH  |  800.00 |
```



### **2）按照查询出的字段的顺序排列**

---

示例

```sql
#关键字`oeder by`后面可以指定具体的字段，也可以写查询出的字段的顺序；
#示例：找出员工的名字和工资，按照员工的工资降序排列；
mysql> select ename,sal from emp order by 2 desc;
+--------+---------+
| ename  | sal     |
+--------+---------+
| KING   | 5000.00 |
| SCOTT  | 3000.00 |
| FORD   | 3000.00 |
| JONES  | 2975.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| ALLEN  | 1600.00 |
| TURNER | 1500.00 |
| MILLER | 1300.00 |
| WARD   | 1250.00 |
| MARTIN | 1250.00 |
| ADAMS  | 1100.00 |
| JAMES  |  950.00 |
| SMITH  |  800.00 |
+--------+---------+
#sal语句当中`2`代表的就是要查询的字段`ename,sal`当中的第二个字段(sal)；
#不建议在实际开发当中使用；
```



## 11、分组查询&having过滤

> 我们可以使用`group by`将字段值相同的记录分为一组，结合分组函数查询；



**单个字段分组查询**

```sql
#查询各个部门的最高薪资；
mysql> select deptno,max(sal) from emp group by deptno order by deptno;
+--------+----------+
| deptno | max(sal) |
+--------+----------+
|     10 |  5000.00 |
|     20 |  3000.00 |
|     30 |  2850.00 |
+--------+----------+ 


```



**多个字段分组查询**

```sql
#找出每个部门不同工作岗位的最高薪资
mysql> select deptno,job,max(sal) from emp group by deptno,job order by deptno;
+--------+-----------+----------+
| deptno | job       | max(sal) |
+--------+-----------+----------+
|     10 | CLERK     |  1300.00 |
|     10 | MANAGER   |  2450.00 |
|     10 | PRESIDENT |  5000.00 |
|     20 | ANALYST   |  3000.00 |
|     20 | CLERK     |  1100.00 |
|     20 | MANAGER   |  2975.00 |
|     30 | CLERK     |   950.00 |
|     30 | MANAGER   |  2850.00 |
|     30 | SALESMAN  |  1600.00 |
+--------+-----------+----------+


```



**注意**

当一条SQL语句当中有`group by`的话，`select`后面只能写分组函数和参与分组的字段；



**Having过滤**

> having用于对group by分组后的数据进行再次筛选；

```sql
#找出每个部门的最高薪资，要求显示薪资大于2900的数据；
#方式一：
mysql> select deptno,max(sal) from emp group by deptno having max(sal) > 2900;
#方式二：
mysql> select deptno,max(sal) from emp where sal > 2900 group by deptno;
```

两种方式查询到的结果都是一样的，但是方式二的效率更高；在实际使用当中，能用where子句提前筛选就使用where，不能使用where子句筛选再考虑使用having；



## 12、连接查询

### 12.1、笛卡尔积

**什么是笛卡尔积？**

笛卡尔积又叫笛卡尔乘积，是一个叫笛卡尔的人提出来的。 简单的说就是两个集合相乘的结果。

假设集合A={a, b}，集合B={0, 1, 2}，则两个集合的笛卡尔积为{(a, 0), (a, 1), (a, 2), (b, 0), (b, 1), (b, 2)}。



**连接查询出现的笛卡尔积现象？**

两张表连接查询，如果未加连接条件的话查询出来的记录条数就是两张表各自记录条数的乘积，也就出现了笛卡尔积。



**如何解决笛卡尔积现象？**

多表查询时添加连接条件，过滤不符合要求的数据。



### 12.2、连接查询分类

- 角度1：从连接条件来说分为：等值连接&非等值连接。

  连接条件出现*等于*就是等值连接，出现*大于*、*小于*等就是非等值连接。

  

- 角度2：从连接对象是否为”自己“分为：自连接&非自连接。

  自链接就是自己连接自己，将一张表当成两张表来用。

  **注意**：自连接必定会导致两张表中字段名重复，这时必须在字段前加上表名前缀加以区分，否则会报错。

  

- 角度3：内连接&外连接。

  - 1、内连接：只查询符合连接条件的记录，不符合的不会被包含在结果集中。

  - 2、外连接：两张表连接查询，有主次之分。主要是查左边的或者右边的表，顺带着将次表中的记录查询出来。主表中记录会被全部查询出来，主表中每一条记录如果在次表中有记录与之匹配则“拼”在一行，没有则的话那一行记录中所有次表字段全部为`NULL`。

  - 3、外连接的分类：左外连接、右外连接、满外连接。

    每一条左/右外连接 都有与之对应的右/左连接。

    满外连接：除了将主表中的记录全部查询出来以外，还将次表中的记录也全部查询出来。

  



### 12.3、SQL92语法实现内外连接

> 注意：MySQL不支持SQL92语法。

**SQL92之内连接**

SQL92语法中内连接就是把连接条件写在`WHERE`关键字之后即可。



**SQL92之外连接**

在 SQL92 中采用（+）代表从（次）表所在的位置。即左或右外连接中，(+) 表示哪个是从表。

```sql
#左外连接 
SELECT last_name,department_name 
FROM employees ,departments 
WHERE employees.department_id = departments.department_id(+); 

#右外连接 
SELECT last_name,department_name 
FROM employees ,departments 
WHERE employees.department_id(+) = departments.department_id; 
```



### 12.3、SQL99语法实现内外连接

**内连接【INNER JOIN】**

```sql
SELECT 字段列表
FROM A表
INNER	JOIN B表 ON	连接条件  
```

SQL99语法的语义更加清晰，使用`JOIN …… ON ……`将表的连接条件与其他过滤条件区分开来。

关键字 JOIN、INNER JOIN、CROSS JOIN 的含义是一样的，都表示内连接。



**外连接【OUTER JOIN】**

外连接又有左、右、满外连接之分。

- 1、左外连接【LEFT OUTER JOIN】

```sql
SELECT 字段列表 
FROM A表 
LEFT JOIN B表 ON 连接条件 
```

- 2、右外连接【RIGHT OUTER JOIN】

```sql
SELECT 字段列表 
FROM A表 
RIGHT JOIN B表 ON 连接条件 
```

由于LEFT、RIGHT这两个关键字已经能区分出一条SQL语句之内连接还是外连接，于是OUTER关键字一般省去。

- 3、满外连接【FULL OUTER JOIN】

满外连接的结果 = 左右表匹配的数据 + 左表没有匹配到的数据 + 右表没有匹配到的数据。 
SQL99是支持满外连接的。使用FULL JOIN 或 FULL OUTER JOIN来实现。 
需要注意的是，MySQL不支持FULL JOIN，但是可以用 LEFT JOIN UNION  RIGHT join代替。



Oracle对SQL92、99语法都支持。



### 12.4、七种JOIN

![image-20220412224143091](D:\Nanum-Note\DataBase\MySQL数据库-img\image-20220412224143091.png)





## 8、 DQL语句执行顺序

一条完整的DQL语句的执行顺序如下：

| 关键字 | 执行顺序 |
| ------ | -------- |
| SELECT | 5        |
| FROM   | 1        |
|        |          |
|        |          |
|        |          |
|        |          |
|        |          |





# 第四章：DML（数据据操作语言）



# 第三章：SQL函数

> 1、SQL 中的函数一般是在数据上执行的，可以很方便地转换和处理数据。
>
> 2、SQL函数分为`内置函数`和我们`自定义的函数`，内置函数的支持情况和实现原理由`DBMS`来决定，因此我们在使用内置的SQL函数时应该注意SQL代码具体的运行环境(DBMS)；
>
> 3、实际上只有很少的函数是被DBMS同时支持，比如：大多数`DBMS`使用运算符 `||`或`+` 来做字符串的拼接，而MySQL数据库使用的是内置函数`CONCAT()`。大部分DBMS都有自己特定的内置函数，这就意味着使用内置SQL函数的SQL代码是`可移植性差`的。



## 1、单行处理函数

> 处理一行，输出一行；
>



**单行处理函数详细**

| 单行处理函数                                                 | 作用                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| lower（name【字段、常量……】）                                | 转换小写；                                                   |
| upper（name【字段、常量……】）                                | 转换大写；                                                   |
| substr（name【字段……】，1【起始下标，从1开始】，1【截取的长度】） | 截取字符串；                                                 |
| length（name【字段、常量……】）                               | 获取长度；                                                   |
| trim（name【字段、常量……】）                                 | 去除空格；                                                   |
| round（sal【数字、字段……】，0【保留小数位的个数，0表示保留整数位，负数顺理往前移】） | 四舍五入；                                                   |
| rand（）                                                     | 生成随机数； 例：0.5942256772836909                          |
| now（）                                                      | 返回当前的日期以及时间，示例：2001-11-01 12:00 ；            |
| ifnull（字段，为空时转换的值）                               | 当字段为NULL的时候将NULL值转换为一个固定的值；该函数用于处理字段可能会出现NULL的情况； |
| case 表达式 when 表达式 then 表达式 when 表达式 then 表达式 else 表达式 end | 就是SQL中的条件选择语句：if…… else if…… else……               |
|                                                              |                                                              |
|                                                              |                                                              |



## 2、聚合函数

> 聚合函数也叫多行处理函数；

示例

```sql
#多行处理函数对每一行的字段进行处理后输出一行；

#示例：统计有多少员工；
mysql> select count(ename) from emp;
mysql> select count(*) from emp;
#区别：`count(*)`统计的是总记录条数与字段无关，`count(ename)`统计的是字段`ename`不为空的数据的个数；

#示例：找出哪个员工的工资最低；
mysql> select min(sal) from emp;

#示例：找出哪个员工的工资最高；
mysql> select max(sal) from emp;

#示例：找出员工的平均工资；
mysql> select avg(sal) from emp;
```



多行处理函数也能组合使用

```sql
mysql> select count(*),min(sal),max(comm) from emp;
```



重点：

- **多行处理函数自动忽略`NULL`！！！**
- 多行处理函数不能直接写在`where`子句后面；
- 使用分组函数但是未使用`group by`语句，那么会将`where`子句查询到的记录自动分为一组，输出一行；









# 第五章：约束

## 1、非空约束



**注意**

非空约束只有列级约束，没有标记约束；

## 2、唯一性约束

> 唯一性约束控制字段不能重复，但是可以为**NULL**；



## 3、主键约束

> 主键约束控制字段不能重复，且能不为**NULL**；
>

### **主键的分类**

- 按照主键字段的数量来区分
  - 单一主键：一个字段作为主键【推荐且常用】
  - 复合主键：多个字段联合起来为一个主键约束【不推荐】
    - 不建议使用，因为违背数据库设计三范式中的第二范式，会产生`部分依赖`；
- 按照主键的性质来区分
  - 自然主键：主键值是和业务没有关系的自然数【推荐】
  - 业务主键：主键值和系统的业务挂钩【不推荐】
    - 例如：银行卡的卡号作为主键、身份证号作为主键……
    - 不推荐使用，因为主键值和系统业务挂钩的话，一旦业务发生变化，主键值可能也会随之发生变化。有的时候可能没法变化因为变化后可能会导致主键值重复；

### **主键自增**





**注意**：一张表的主键约束只能有一个，主键字会自动添加索引`index`；

## 4、外键约束

外键约束主要是用来做关联查询，使用外键约束可以将表与表之间建立联系；





# 第六章：数据库设计三范式【重点】

## 1、什么是三范式？

三范式就是设计数据库表的**依据**。依据三范式设计出来的表不会出现冗余数据；

## 2、三范式都有哪些？

- 第一范式：任何一张表都应该有主键，并且每一个字段原子性不可再分；

- 第二范式：建立在第一范式的基础之上，所有非主键字段完全依赖主键字段，不能产生部分依赖；

  复合主键就违反了第二范式，会产生数据冗余；

  **多对多，三张表，关系表加两个外键；**

- 第三范式：建立在第二范式的基础之上，所有非主键字段直接依赖主键，不能产生传递依赖；

  **一对多，两张表，多的加外键；**
  
  

**注意**：三范式只是**理论**上的方法，而在实际开发过程（**实践**）当中不一定要完全遵守三范式，因为完全遵守三范式会导致创建很多张表，多张表连接查询会导致查询速度变慢（连接查询产生笛卡尔积）。这时候为了满足客户的需求，会选择空间（冗余数据）来换速度；

## 3、一对一关系的设计方案？

> 一对一的设计方案有两种，分别为：1主键共享、2外键唯一；

**主键共享【不推荐】**

第二张表的主键添加外键，引用第一张表的主键，就是主键共享；

**外键唯一【推荐】**

两张表，多的一方额外添加一个字段，这个字段添加唯一外键；





# 第五章：注意事项
- 每一条SQL语句都是以分号（`;`）结束的，不见分号（`;`），sql语句不会执行；

- SQL语句不区分大小写；

- 在MySql当中，字符串用单引号或者双引号括起来都可以，但是在Oracle、Sql Server等数据库当中只能使用单引号；尽管在MySql数据库当中可以使用双引号，但是为了保证sql语句的通用性，建议字符串都使用单引号括起来；

- 在数据库当中只要数学表达式当中有`NULL`参与，结果必定是`NULL`；

 

  

# 第七章：MySQL配置

## 1、字符集与比较规则的配置

### 1）查看字符集与比较规则

- **查看MySQL默认的字符集**

  ```sql
  # 以下为MySQL8.0的默认字符集
   mysql>  show variables like 'character%';
  +--------------------------+----------------------------------+
  | Variable_name            | Value                            |
  +--------------------------+----------------------------------+
  | character_set_client     | utf8mb4                          |
  | character_set_connection | utf8mb4                          |
  | character_set_database   | utf8mb4                          |
  | character_set_filesystem | binary                           |
  | character_set_results    | utf8mb4                          |
  | character_set_server     | utf8mb4                          |
  | character_set_system     | utf8mb3                          |
  | character_sets_dir       | /usr/local/mysql/share/charsets/ |
  +--------------------------+----------------------------------+
  8 rows in set (0.00 sec)
  ```
  
- **查看MySQL默认的比较规则**

  ```sql
  # 以下为MySQL8.0的默认比较规则
  mysql>  show variables like 'collation%';
  +----------------------+--------------------+
  | Variable_name        | Value              |
  +----------------------+--------------------+
  | collation_connection | utf8mb4_0900_ai_ci |
  | collation_database   | utf8mb4_0900_ai_ci |
  | collation_server     | utf8mb4_0900_ai_ci |
  +----------------------+--------------------+
  3 rows in set, 1 warning (0.00 sec)
  ```



### 2）配置字符集和比较规则