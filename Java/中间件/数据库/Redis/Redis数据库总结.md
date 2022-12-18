# Redis数据库总结.md

# 一、简介

### 基本内容

1、NoSQL数据库
2、存储数据的结构是key:value形式的
3、是一个缓存服务器
4、读写速度是非常的高
5、解决session共享的问题
6、解决分布式锁的问题



### 中文官网

可以在[中文官网](https://www.redis.net.cn/)查Redis的命令。



# 二、常用命令

### Redis存储的五种数据结构

```
1、字符串（string） key ---> value
2、哈希（hash） key --> filed value
3、列表（list） key --> v1 v2 v2 v3 v3……
4、无序集合（set） key--> v1 v2 v3……
5、有序集合（zset） key --> score v1 s2 v2
```



### string常用命令

- 添加单个值（set）

```
set key value
例如：set name lisi
说明：已经存在的key，value会被覆盖。
```



- 获取单个值（get）

```
get key
例如：get name
```



- 批量添加值（mset）

```
mset key1 value1 key2 value2 ……
例如：mset name lisi age 20 email lisi@qq.com
```



- 批量获取值（mget）

```
mget key1 key2 ……
例如：mget name age email
```



- 自增（incr）、自减（decr）

```
自增：incr key
自减：decr key
例如：incr age、decr age
```



- 自增/自减指定量

```
自增：incrby key increment（增量）
自减：decrby key decrement（少量）
例如：incrby age 10（年龄自增10岁）、decrby age 10（年龄自减10岁）
```



- 查询所有的键（key）

```
keys *
```



- 刷新数据库（清空所有的值）

```
flushall
```



- 添加一个值的同时设置存活时间（setex）

```
setex key seconds value
例如：setex name 5 lisi（5秒之后name就获取不到了）
```



- 查看一个值的剩余存活时间（ttl）

```
ttl key
例如：ttl name 
返回‘-1’表示永久，返回‘-2’表示过期，正数表示剩余存活时间。
```



- 添加单个值（不会覆盖）

```
setnx key value
例如：setnx name lisi
返回’1‘表示添加成功，返回'0'表示添加失败。
```



- 追加值（append）

```
append kay value
例如：append name 1234
返回追加后的字符串长度。
```



- 查看字符串的长度（strlen）

```
strlen key
例如：strlen name
返回key为name的值的长度，返回‘0’表示key不存在。
```



- 删除key（del）

```
del key
例如：del name
返回‘1’表示删除成功，‘0’表示删除失败。
```



# 三、通用命令(generic)

### 1、查看符合模板的所有key

```shell
KEYS pattern
```

📄**示例**

```shell
# 查看所有key
keys * 
```

🚨**警告**

不建议在生产环境下使用，因为这种查询是模糊查询，效率不高。当Redis数据量很大的时候使用这种命令查询会变得非常慢，并且Redis是单线程的，其他请求会被阻塞。



### 2、删除一个或多个key

```
DEL key [key ...]
```

📄**示例**

```
127.0.0.1:6379> DEL k1 k2 k3 k4
(integer) 3
```



### 3、判断key是否存在

```
EXISTS key [key ...]
```

**📄示例**

```
127.0.0.1:6379> exists k1
(integer) 0（返回0表示不存在，返回1表示存在）
```



### 4、给key设置有效期

> 给key设置有效期，当key过期时会被自动删除。

```
EXPIRE key seconds
```

**📄示例**

```
127.0.0.1:6379> expire k1 10000
(integer) 1（返回1表示设置成功，返回0表示设置失败）
```



### 5、查看key的剩余时间（有效期）

```
TTL key
```

**📄示例**

```
127.0.0.1:6379> ttl k1
(integer) 9724（返回-1代表永久有效，返回-2表示key已经过期被删除）
```





# 四、String类型常用命令

