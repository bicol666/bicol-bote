# 一、安装Docker

1、更新`yum`包

```shell
yum update
```

2、安装需要的软件包，`yum-util`提供`yum-config-manager`功能，另外两个是`devicemapper`驱动依赖的

```shell
yum install -y yum-utils device-mapper-persistent-data lvm2
```

3、配置`yum`源

```shell
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

4、安装`docker`，出现输入的界面都按`y`

```shell
yum install -y docker-ce
```

5、查看`docker`版本，验证是否验证成功。

```shell
docker -v
```



# 二、Docker架构

·镜像（lmage）：Docker镜像（lmage），就相当于是一个root文件系统。比如官方镜像 ubuntu：16.04就包含了完整的一套Ubuntu16.04最小系统的root文件系统。

·容器（Container）：镜像（lmage）和容器（Contain er）的关系，就像是面向对象程序设计中的类和对象一样，镜像是静态的定义，容器是镜像运时的实体。容器可以被创建、启动、停止、删除、暂停等。

·仓库（Repository）：仓库可看成一个代码控制中心，用来保存镜像。

# 三、Docker相关命令

### 1、服务相关命令（daemon）

- 开启`docker`

  ```shell
  systemctl start docker
  ```

- 停止`docker`

  ```shell
  systemctl stop docker
  ```

- 重启`docker`

  ```shell
  systemctl restart docker
  ```

- `Linux`开机启动`docker`服务

  ```shell
  systemctl enable docker
  ```



### 2、镜像相关命令（image）

- 查看镜像

  ```shell
  docker images
  ```

  - 查看所有的镜像ID

    ```shell
    docker images -q
    ```

- 搜索镜像

  ```shell
  docker search [镜像名称]
  ```

- 拉取（下载）镜像

  ```shell
  docker pull [镜像名称]:[版本号]
  ```

  不指定版本号默认最新版本。

- 删除镜像

  - 方式一，通过IMAGE ID删除

  ```shell
  docker rmi [IMAGE ID]
  ```

  - 方拾二，通过镜像名:版本号删除

  ```shell
  docker rmi [镜像名]:[版本号]
  ```

  - 删除所有镜像

    ```shell
    docker rmi `docker images -q`
    ```



·进入容器
docker exec 参数#退出容器，容器不会关闭
·停止容器
docker stop容器名称
·启动容器
docker start容器名称
·删除容器：如果容器是运行状态则删除失败，需要停止容器才能删除dockerrm 容器名称
·查看容器信息
docker inspect容器名称





·查看容器
docker ps#查看正在运行的容器
docker ps-a#查看所有容器
·创建并启动容器
docker run 参数
参数说明：
·-i：保持容器运行。通常与-t同时使用。加入it这两个参数后，容器创建后自动进入容器中，退出容器后，容器自动关闭。
·-t：为容器重新分配一个伪输入终端，通常与-i同时使用。
·-d：以守护（后台）模式运行容器。创建一个容器在后台运行，需要使用dockerexec进入容器。退出后，容器不会关闭。
·-it创建的容器一般称为交互式容器，id创建的容器一般称为守护式容器
·--name：为创建的容器命名。



# 五、Docker应用部署

### 1、MySQL

```shell
docker run -id \
-p 3306:3306 \
--name=c_mysql \
-v $PWD/conf:/etc/mysql/conf.d \
-v $PWD/logs:/logs \
-v $PWD/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=bicolmsMS13 \
mysql:8.0.30
```

### 2、Tomcat

```shell
docker run -id --name=c_tomcat \
-p 3333:8080 \
-v $PWD:/usr/local/tomcat/webapps \
tomcat:9-jdk17
```



### 3、nginx

```shell
docker run -id --name=c_nginx \
-p 80:80 \
-v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf \
-v $PWD/logs:/var/log/nginx \
-v $PWD/html:/usr/share/nginx/html \
nginx
```



### 4、Redis

```shell
docker run -id --name=c_redis -p 6378:6379 redis
```

