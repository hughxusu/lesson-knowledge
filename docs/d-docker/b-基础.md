# 基本使用

Docker使用的基本流程

```mermaid
graph LR

a(搜索镜像)-->b(下载镜像)-->c(启动容器)
```

## 找到合适的镜像

[Docker hub](https://hub.docker.com/)是目前全球最大的容器镜像托管平台，用户可以在上面的到需要的镜像。

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/docker/Xnip2026-04-14_19-43-31.jpg" style="zoom:60%;" />

找到镜像后选择可以选择合适的版本

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/docker/Xnip2026-04-14_19-41-01.jpg" style="zoom:60%;" />

### 镜像操作

在命令行中搜索镜像，命令行搜索不会走镜像加速通道，所以搜索结果可能超时

```shell
docker search nginx
```

下载镜像最新版本镜像

```shell
docker pull nginx
docker pull nginx:latest
```

下载指定版本的镜像

```shell
docker pull nginx:alpine
```

查看下载成功的镜像

```shell
docker images
```

查看镜像的历史，可以查看安装的PostgreSQL，具体是哪个版本。

```shell
docker history nginx:alpine
```

移除已下载的镜像

```shell
docker rmi nginx:alpine
```

## 启动容器

容器是镜像的实例，镜像相当于软件安装包，容器相当于安装好的软件。用户的最终目标是使用软件，启动容器的过程可以看做是在安装软件。

使用`docker run`命令可以启动容器，启动Nginx镜像

```shell
docker run nginx
docker run nginx:latest
```

> [!warning]
>
> 启动后不要关闭窗口，否则镜像会停止。

启动另外的窗口可以查看运行中的容器

```shell
docker ps
```

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/docker/Xnip2026-04-15_13-35-21.jpg" style="zoom:80%;" />

查看容器的其他命令

```shell
docker ps -a   # 查看所有容器，包括已经停止运行的

```

启动已经停止的容器

```shell
docker start [容器id]
```

停止运行的容器

```shell
docker stop [容器id]
```

重启容器，类似与电脑重新启动

```shell
docker restart [容器id]
```

查看容器占用资源

```shell
docker stats [容器id]
```

查看容器日志，可以监控容器的使用情况

```shell
docker logs [容器id]
```

删除容器

```shell
docker rm [容器id]       # 一般删除，必须先将容器停止
docker rm -f [容器id]    # 强制删除，可以直接删除运作中的容器
```

### `run`命令的使用

`docker run`命令的格式为

```shell
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

* `[OPTIONS]`配置容器运行时的参数。
* `[COMMAND]`这是容器启动后默认执行的第一个程序。
* `[ARG...]`这是传递给的`[COMMAND]`具体参数。
* 对于服务型容器，镜像在制作时，已经将启动命令写入，`[COMMAND]`和`[ARG...]`不常用，除非必须改变镜像的默认启动行为。

后台启动服务

```shell
docker run -d --name demo-nginx nginx
```

* `-d`表示后台启动服务程序。
* `--name demo-nginx`用户指定容器的名字，不会再随机生成。
* `nginx`使用的镜像。

使用端口映射

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/docker/1681604245712.jpeg" style="zoom:70%;" />

```··shell
docker run -d --name demo-nginx -p 8080:80 nginx:latest 
```

* `-p 8080:80`增加容器端口映射`8080`本机端口，`80`容器端口。

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/docker/Xnip2026-04-15_14-21-15.jpg" style="zoom:85%;" />
