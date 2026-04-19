# 存储与网络

## 存储映射

存储在容器中的数据，随着容器的删除，数据会一同销毁。为了实现数据的持久化，或者让容器与宿主机共享文件，Docker 提供了存储映射机制。

### 绑定挂载

将宿主机上**任意**的一个具体路径映射到容器内部。

* 直接操作宿主机文件系统，性能极高。
* 可以随时修改挂载的内容，容器内会实时生效。

适用场景：配置文件挂载。

```shell
docker run -d -p 8080:80 -v ./app/nginx-html:/usr/share/nginx/html/ --name nginx-app nginx:latest
```

* `-v`存储操作。
* `./app/nginx-html`宿主机的路径。
* `:/usr/share/nginx/html/`映射到容器中的路径。

修改了`./app/nginx-html`下的文件，可以直接影响Nginx服务器。

### 容器卷

卷是由Docker完全管理的存储空间，存储在宿主机的特定目录下，用户一般不需要关心它在硬盘的具体位置，其统一保存在`/var/lib/docker/volumes`目录下。

* 可以通过`docker volume`命令进行创建、备份和迁移。
* 支持多个容器同时挂载。

适用场景：数据库持久化、跨容器共享数据。

> [!note]
>
> 容器卷是最常用的存储映射方式。

```shell
docker run -d --name mysql-app -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456  -v mysql-data:/var/lib/mysql  -v ./app/mysql/conf:/etc/mysql/conf.d -v ./app/mysql/logs:/var/log/mysql mysql
```

* `-e MYSQL_ROOT_PASSWORD=123456`设置root用户的密码。
* `-v mysql-data:/var/lib/mysql`将数据库的数据存储映射为数据卷。
* `-v ./app/mysql/conf:/etc/mysql/conf.d`将数据库的配置文件夹挂载到指定的文件夹。
* `-v ./app/mysql/logs:/var/log/mysql`将MySQL的日志文件夹挂载到指定的文件夹。

查看卷映射

```shell
docker volume ls
```

* `volume`容器卷操作，操作的容器卷都是有Docker管理的。

查看数据卷详情

```shell
docker inspect mysql-data
```

删除容器后，数据卷应然存在

```shell
docker volume rm mysql-data
```

数据卷也可以单独创建

```shell
docker volume create app-data
```

上面创建的数据卷，可以挂载在多个容器上，可以实现多个容器之间的数据共享。

## 网络

容器的端口可以映射到宿主机上，这样外部访问通过宿主机的端口，就可以访问容器的内容。

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/docker/docker-01-4-.png" style="zoom:45%;" />

> [!think]
>
> 同一个宿主机内的容器，如何通过网络来进行访问呢？

创建两个镜像

```shell
docker run -d --name nginx-one -p 8080:80 nginx
docker run -d --name nginx-two -p 8090:80 nginx
```

进入容器`nginx-one`，测试通过`curl`命令实现容器的互相访问。

```shell
docker exec -it nginx-one bash
```

在Docker的默认网络模型中， 存在一个`docker0`网桥连接所有容器与主机网络。

* 每个容器都有自己的虚拟接口，一端位链接容器，另一端连接`docker0`。
* 容器之间可以通过`docker0`通信。
* 容器也可以通过`docker0`链接主机网络，与外部通信。

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/docker/docker-01-11-.png" style="zoom:45%;" />

使用`ip a`命令可以查看所有网卡信息

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/docker/Xnip2026-04-19_09-38-41.jpg" style="zoom:65%;" />

查看Docker中存在的网络

```shell
docker network ls
```

* `network`操作Docker网络功能。
* `bridge`默认桥接网络，即`docker0`网络。
* `host`共享宿主机的网络，但没有网络隔离，端口容易冲突。
* `none`封闭网络，完全断网，用于极高安全要求的本地计算。

查看容器的详细信息

```shell
docker inspect nginx-two
```

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/docker/Xnip2026-04-19_11-48-34.jpg" style="zoom:60%;" />

进入容器`nginx-one`，测试通过`curl`命令通过容器`docker0`网络的IP访问`nginx-two`的`80`端口，实现网络互访。

### 自定义网络

用户可以自由创建Docker内部网络

```shell
docker network create app-net
docker network create -d bridge app-net
```

* 上面两个命令等价，创建的默认网络就是桥接网络。

重新启动容器将容器加入自定义网络

```shell
docker run -d --name nginx-one -p 8080:80 --network app-net nginx
docker run -d --name nginx-two -p 8090:80 --network app-net nginx
```

* `--network app-net`将容器加入指定网络。

> [!note]
>
> 加入自定义网络后，可以通过容器名实现网络之间的访问，如果是`docker0`不可以。

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/docker/docker-01-10-.png" style="zoom:45%;" />

## 最佳实践

```shell
docker run 
-d 
--name mysql-app 
-p 3306:3306 
-v mysql-data:/var/lib/mysql 
-v ./app/mysql/conf:/etc/mysql/conf.d 
-v ./app/mysql/logs:/var/log/mysql mysql
--network app-net
-e MYSQL_ROOT_PASSWORD=123456  
```

启动容器过程中需要使用的命令

1. `-d`一般容器都是后台启动。
2. `--name`设置容器的名称。
3. `-p`指定外界访问端口。
4. `-v`配置文件、日志和环境变量等数据映射到文件夹中。
5. `-e`启动容器时需要传入的启动参数，根据不同的容器，单独设置。
6. `-v`持久化数据映射为容器卷，可以由Docker同一管理。
7. `--network`将容器加入质监网络。



