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

查看命令帮助使用`--help`

```shell
docker images --help
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

批量操作

```shell
docker ps -aq # -a显示所有容器，-q只显示容器ID
docker stop $(docker ps -aq)  # 停止所有的容器
docker rm $(docker ps -aq)  # 删除所有的容器
docker rm -f $(docker ps -aq)  # 强制删除所有容器
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

* [阿里云需要设置服务器IP和端口的号访问权限](/docs/a-linux/c-服务器?id=域名和端口号)，可以访问Nginx服务。

## 创建个人镜像

修改Nginx容器中的访问页，作为新的镜像部署

1. 进入容器容器内部操作

```shell
docker exec -it demo-nginx /bin/bash
```

* `exec`在已启动的容器中执行一个命令。
* `-i`交互模式；`-t`使用虚拟终端。
* `demo-nginx`要进入的容器名；`/bin/bash`在容器中执行的程序。

2. 进入服务器页面位置，服务页面为位置符合Nginx服务器的规范。

```shell
cd /usr/share/nginx/html/
```

3. 修改`index.html`页面内容

```shell
echo '<h1>Hello, docker!</h1>' > index.html
```

4. `exit`命令，从容器中退出。
4. 根据容器生成镜像

```shell
docker commit -m 'demo nginx image' demo-nginx my-nginx:1.0
```

* `-m 'demo nginx image'`导出镜像的说明信息。
* `demo-nginx`容器的名称。
* `my-nginx`制作镜像的名称。
* `:1.0`镜像的版本标识。

5. 将镜像导入到文件

```shell
docker save my-nginx:1.0 > ./my-nginx.tar
```

* `my-nginx:1.0`要导出的镜像名称。
* `> ./my-nginx.tar`导出的路径和文件。

6. 将文件导入为镜像

```shell
docker load < ./my-nginx.tar 
```

* `< ./my-nginx.tar`导入为镜像的文件。

### 华为云SWR

Docker hub服务器位于国外，ECS或国内网络经常无法链接，华为云提供的SWR镜像服务器，可以帮助用户保存私人镜像用于项目部署。登录华为云后搜索swr

![](https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/docker/Xnip2026-04-17_14-23-56.jpg)

为镜像仓库创建组织

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/docker/Xnip2026-04-17_14-35-08.jpg" style="zoom:90%;" />

生成登录指令，并在服务器终端中输入登录指令，可以登录SWR上传镜像

![](https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/docker/Xnip2026-04-17_14-41-29.jpg)

镜像重命名

```shell
docker tag my-nginx:1.0 swr.cn-north-4.myhuaweicloud.com/hughxusu/my-nginx:1.0
```

* `my-nginx:1.0`原始镜像名。
* `swr.cn-north-4.myhuaweicloud.com/`上传服务器
* `hughxusu/my-nginx:1.0`新镜像名。

> [!warning]
>
> 向镜像服务器上传镜像，镜像名格式为`服务器路径/组织名/镜像名:版本号`，如果没有服务器路径，默认上传到Docker hub。

上传镜像文件

```shell
docker push swr.cn-north-4.myhuaweicloud.com/hughxusu/my-nginx:1.0
```
