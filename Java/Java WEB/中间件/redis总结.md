# Redis总结

# 一、简介

> 不同的数据库底层的存储机制不同，也就擅长存储不同的数据，导致有不同的适用场景；

- **Re**mote **di**ctionary **s**erver(远程字典服务器)是一种由`C语言`编写的、开源的、**基于内存运行**且支持持久化的、高性能的**No SQL**(Not only SQL)数据库。是当前最热门的NoSQL数据库之一；

- Redis采用K-V模型存储数据；

- Redis中的数据大部分时间都是存储在内存当中的（访问效率高）；

  适合存储：`频繁访问`、`数据量较小`（内存太贵，数据量太大放不下）的一类数据；

  因此，Redis数据库主要用来做缓存；



## 特点

1、支持持久化

2、支持多种数据结构

3、支持数据备份



# 二、Linux环境下安装Redis

**安装步骤**

- **下载安装包**

  去[官网](https://redis.io)下载安装包，下载好后上传到Linux系统里；

- **解压安装包**

  解压安装包 tar -zxvf……

- **编译src目录** make

  在编译之前需保证已安装c语言编译器`gcc`！

- **安装Redis** make install



# 三、常用命令

#### **启动Redis服务**

-  前台启动

  ```shell
  redis-server
  ```

  

- 后台启动

  ```shell
  redis-server	&
  ```

- 启动时指定配置文件

  ```
  redis-server	redis.conf	&
  ```

  参数说明：

  - redis.conf：redis配置文件的路径；
  - &：可选参数，后台启动；



#### 关闭Redis服务

**1、通过kill命令暴力关闭（不推荐）**

查询到redis的pid；

```shell
ps -ef | grep redis
```

通过kill命令暴力关闭；

```shell
kill -9 pid
```

这种方式会直接把redis的进程杀死，这时内存里的数据可能还未存储到磁盘当中，会造成数据丢失，因此不推荐；

**2、通过Redis命令关闭**

```
redis-cli	shutdown
```

通过redis客户端向服务端发送关闭的指令，数据安全；





