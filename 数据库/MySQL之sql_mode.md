# MySQL之sql_mode.md

# 一、什么是sql_mode

MySQL的`sql_mode`就是MYSQL对数据校验的策略，在`严格模式`下MySQL会对数据进行严格的校验，以保证数据的合法性、可靠性。比如说：当一个`tinyint`类型的字段我们插入超出`1`个字节范围的数字时，在严格模式下就会报错。

# 二、查看MySQL数据库的sql_mode

方式一：查看数据库配置中变量名包含mode的配置参数，`%`匹配多个任意字符，`_`匹配单个任意字符。

```mysql
show variables like "%mode%";
```

```mysql
+--------------------------+------------------------+
| Variable_name            | Value                  |
+--------------------------+------------------------+
| block_encryption_mode    | aes-128-ecb            |
| gtid_mode                | OFF                    |
| innodb_autoinc_lock_mode | 2                      |
| innodb_strict_mode       | ON                     |
| offline_mode             | OFF                    |
| pseudo_replica_mode      | OFF                    |
| pseudo_slave_mode        | OFF                    |
| rbr_exec_mode            | STRICT                 |
| replica_exec_mode        | STRICT                 |
| slave_exec_mode          | STRICT                 |
| sql_mode                 | NO_ENGINE_SUBSTITUTION | 这里就是sql_mode
| ssl_fips_mode            | OFF                    |
+--------------------------+------------------------+
12 rows in set, 1 warning (0.04 sec)
```



方式二：

```mysql
select @@sql_mode;
```

```mysql
+------------------------+
| @@sql_mode             |
+------------------------+
| NO_ENGINE_SUBSTITUTION |
+------------------------+
```



# 三、修改MySQL的sql_mode为严格模式

- Windows：

  1、对当前会话有效：

  ```mysql
  set sql_mode='STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
  ```

  2、全局配置：在MySQL的安装目录下找到：`/etc/my.cnf`进行修改。

  ```mysql
  # 开启严格模式
  sql-mode="STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"
   
  # 关闭严格模式
  sql-mode="NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"
  ```

- Linux

  找my.cnf文件 修改对应的配置（同上）。

