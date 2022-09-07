# 一、JDK环境搭建



**环境变量的配置**

1. 编辑profile文件：`vi /etc/profile`

2. 在末尾追加以下内容：

```powershell
export	JAVA_HOME=/usr/local/jdk-17.0.1

export	PATH=$JAVA_HOME/bin:$PATH

export	CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/jre/lib/rt.jar
```

3. profile文件修改完成，执行`source /etc/profile` 让上面的配置生效；

   source命令通常用于重新执行刚修改的初始化文件，使之立即生效，而不必注销并重新登录；
   语法：`source	文件名`

4. 检测环境变量是否成功

   使用`java -version`命令检测是否配置成功；

![image-20211102224848941](C:\Users\nanum008\AppData\Roaming\Typora\typora-user-images\image-20211102224848941.png)

​		出现以上界面这说明配置环境变量成功！



# 二、Tomcat环境搭建

## 1. 文件解压缩

首先将归档文件解压缩到`/usr/local/`，使用命令：`tar -zxvf apache-tomcat-9.0.54.tar.gz -C /usr/local`



## 2. 启动&关闭Tomcat

启动：在tomcat的bin目录下输入`./startup.sh `即可；

关闭：在tomcat的bin目录下输入`./shutdown.sh `即可；



## 3.  打开防火墙,使外部能访问

防火墙运行时可能会导致外部不能访问

未开启防火墙请忽略



# 三、MySQL配置

mysql默认安装目录：/usr/local/mysql

mysql存储数据库文件目录/usr/local/mysql/data

mysql默认占用端口：3306



## 1. 安装前准备工作

1）MySQL安装包下载地址：https://dev.mysql.com/downloads/mysql/

2）在安装之前检测是否安装过MySQL及其分支，避免冲突；

- 检测是否安装过MySQL

```
rpm -qa | grep mysql
```

- 检测是否存在mariadb-libs分支

```powershell
yum list | grep mariadb-libs
```

- 删除mariadb-libs分支

```powershell
yum remove mariadb-libs
```



## 2. 安装包解压缩

 将安装包解压缩到：/usr/local/

```
tar -zxvf 安装包名路径 -C /usr/local/ 
```

值得注意的是如果是最新的`xz`格式文件，参数`-zxvf`应该改为：`-xvf`，否者会报错；



可选项：重命名MySQL安装目录名

切换到安装包解压目录所在的父目录执行以下命令：

```
mv mysql-5.6.23-linux-glibc2.5-x86_64  mysql-5.6.23
```



可选项：删除安装包文件，节省磁盘空间；

```
rm mysql-5.6.23-linux-glibc2.5-x86_64.tar.gz
```



## 3. 创建Mysql用户组并添加Mysql用户

在Linux系统中，很多软件的运行需要依赖特定的用户组，而MySQL软件的运行就需要依赖MySQL用户及用户组；

1)添加Mysql用户以及用户组

```
useradd -r -s /sbin/nologin mysql
```

这里创建mysql用户的时候未指定用户组，系统会自动创建一个同名的用户组；



2）修改目录权限

- 进入Mysql安装目录：

```
cd /usr/local/mysql
```

- 修改目录的拥有者:

```
chown -R mysql ./
chgrp -R mysql ./
```

这里的点`./`代表的就是当前目录，选项`-R`表示递归当前目录及其子目录；



3)检测是否授权成功

```powershell
#ll /usr/local/mysql
```

查看文件的详细信息即可查看mysql安装目录下所有的文件的用户组信息；





## 4. 初始化MySQL安装并启动Mysql服务

1)初始化mysql

- 切换到mysql的bin目录下：

```powershell
cd /usr/local/mysql/bin
```

- 执行mysql初始化文件

```powe
[root@iZuf6cnjz7d248ewinn4bxZ bin]# ./mysqld --initialize --user=mysql --datadir=/usr/local/mysql/data --basedir=/usr/local/mysql

2021-11-03T15:19:50.222619Z 0 [System] [MY-013169] [Server] /usr/local/mysql/bin/mysqld (mysqld 8.0.27) initializing of server in progress as process 41612
2021-11-03T15:19:50.313335Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2021-11-03T15:19:52.623706Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2021-11-03T15:19:54.025465Z 0 [Warning] [MY-013746] [Server] A deprecated TLS version TLSv1 is enabled for channel mysql_main
2021-11-03T15:19:54.025490Z 0 [Warning] [MY-013746] [Server] A deprecated TLS version TLSv1.1 is enabled for channel mysql_main
2021-11-03T15:19:54.110301Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: 3;,RCm)rbe#x
```

可以看见初始化密在最后一行，请务必牢记；

**注意！在此之前不要手动创建data文件夹，否则会出现data已存在，初始化失败的情况。如果创建了或者已经初始化过但失败了，可以删除data文件夹重新初始化。**



- 进入`/var/lock/subsys/`目录下删除mysql文件；

```powershell
cd  /var/lock/subsys/
rm -f mysql
```

**注意：必须删除这个文件，否者mysql无法正常启动！**



2)启动mysql服务

```powershell
service mysql start
```

**注意：如果报错（The server quit without updating PID file ），可能是进程中有mysql的进程，上次的退出并没有自动结束该pid，导致新的进程无法启动。运行命令 ps -ef |grep mysql 找到mysql的进程结束它，然后再启动mysql服务即可。**



## 5. 配置环境变量

1）打开profile文件

```
vim /etc/profile
```

2)在末尾追加以下内容

```
 export MYSQL_HOME=/usr/local/这里填Mysql的安装目录名
 export PATH=$PATH:$MYSQL_HOME/bin
```

保存并退出：按esc键退出编辑模式，输入`:wq`保存并退出；

3）执行以下命令使配置生效

```
source /etc/profile
```



## 6. 修改ROOT用户密码

当我们使用初始密码登录mysql后输入sql命令时会报以下错误，这是由于我们未修改root初始密码导致的；

```
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
```

修改命令：

```sql
alter user 'root'@'localhost' identified by '要修改的密码';
```



## 6. 配置开机自启动

```powershell
ln -s /usr/local/mysql/support-files/mysql.server /etc/init.d/mysql
ln -s /usr/local/mysql/bin/mysql /usr/bin/mysql
```



## 7. 配置MySQL服务远程可访问

1）登录Mysql

```
mysql -uroot -p
```

会提示输入管理员密码。注意，这里输入的密码不会回显。



2)授权root 用户远程连接服务器

```sql
use mysql;
--这表示任何ip都可以访问root用户；
update user set host = '%' where user = 'root';
--刷新操作
flush privileges;
```

