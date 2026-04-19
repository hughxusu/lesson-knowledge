## 容器命令

### 基本操作

```shell
docker rmi -f hello-world:latest mysql:5.6 # 删除多个镜像
docker rmi -f $(docker images -qa) # 删除全部镜像

docker ps -l # 查看上一个容器
docker ps -n 3 # 查看上3次运行的容器
docker ps -lq # 只显示上一次的容器编号
docker top # 容器内运行的进程

docker start -i b9c025a4d557 # 已交互方式重启容器

# 以ctrl+p+q退出后可以使用
docker attach [容器id] # 进入退出后没有停止的容器

docker kill [容器id] # 强制停止

docker cp [容器id]:/tmp/yum.log ./ # 将容器内的数据拷贝到容器外
docker cp ./index [容器id]:/tmp/ # 将容器外部的文件复制到容器内部

# 暂停容器
docker pause [容器id]

# 暂停容器继续执行
docker unpause [容器id]


# 删除所有不使用的镜像
docker image prune --force --all

# 删除所有停止的容器
docker container prune -f
```

## DockerFile

使用Dockerfile创建Image流程：

1. 创建文件夹，并在文件夹创建`Dockerfile`文件；
2. 使用docker build命令创建docker镜像。

```shell
docker build -f [DockerFile] -t [镜像名] . # 如果在当前文件夹下，可以省略-f
docker build -t nginx:v3 . # 当前文件夹包含唯一Dockerfile文件
```

3. 使用`run`命令执行镜像文件。

Dockerfile基础知识

* 每条保留字子类必须为大写字母且后面要跟随至少一个参数
* `#` 表示注释，必须单独一行
* 每条指令创建一个新的镜像层，对镜像进行提交，相当于每条指令对镜像进行一次`commit`

执行流程

1. 从基础镜像运行一个容器
2. 执行一条指令对容器进行修改
3. 执行类似于commit操作提交一个新镜像层
4. docker基于上一个提交在运行一个新容器
5. 直到全部指令执行完成

### 保留关键字

* `FROM`当前镜像从从哪个基础镜像开始。`FROM scratch`表示最基础镜像。
* `MAINTAINER`作者和作者邮箱
* `RUN`容器构建时需要运行的命令，主要是linux shell命令
* `EXPOSE`当前容器对外暴露的端口号
* `WORKDIR`指定在创建容器后，终端默认登录的工作目录(落脚点目录)
* `ENV`用来在构建镜像过程中设置环境变量
* `ADD`拷贝文件或文件夹进入镜像，且会自动处理url和解压tar压缩包
* `COPY`拷贝文件或文件夹进入镜像
* `VOLUME`容器卷数据，用于保存数据和持久化

```do
VOLUME ["/dataVolumeContainer1", "/dataVolumeContainer2"]
```

* `CMD`指定一个容器启动时要运行的命令，只有最后一个生效。会被docker run后面的参数替换掉。
* `ENTRYPOINT`指定一个容器启动时要运行的命令，追加命令，该命令不会被docker run后面的参数替换。
* `ONBUILD`当构建一个被继承的DockerFile时运行命令，父镜像在被子继承后父镜像的onbuild被触发。

说明：

1. `CMD`和`ENTRYPOINT`只能设置一个，都是在最后生效。
2. 使用`ENTRYPOINT`可以在docker run时追加命令，实现启动自定义参数。

### 镜像实例

```dockerfile
# 实例1
FROM centos
MAINTAINER hughxusu<hughxusu@qq.com>

ENV MYPATH /usr/local # 设置一个环节变量
# 可以直接写目录
WORKDIR $MYPATH # 引用环境变量

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80
CMD /bin/bash

# 实例2
FROM centos

RUN yum install -y curl
ENTRYPOINT [ "curl", "-s", "http://ip.cn"] # 使用 docker run ip -i 运行镜像时，命令会被追加到 curl -s -i http://ip.cn 上
ONBUILD RUN echo "father onbuild--------886"  # 继承该镜像的镜像制作开始会调用该命令

# 实例3
FROM centos
MAINTAINER hughxusu<hughxusu.qq.com>

COPY c.txt /usr/local/cincontainer.txt
ADD jdk.tar.gz /usr/local
ADD apache.tar.gz /usr/local

RUN yum -y install vim

ENV MYPATH /usr/local
WORKDIR $MYPATH

ENV JAVA_HOME /usr/local/jdk
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV PATH $PATH:$JAVA_HOME/bin

EXPOSE 8080
CMD /usr/local/apache/startup.sh && tail -F /usr/local/apache/bin/logs/log.out
```

## 安装常用软件

### 使用sebp/elk

* 说明文档<https://elk-docker.readthedocs.io/#persisting-log-data>

```shell
# 内存不足，修改虚拟内存，在/etc/sysctl.conf文件最后一行增加
vm.max_map_count=262144
# 执行命令
sysctl -p

# 目前使用版本容器命令
sudo docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -v eplugins:/opt/elasticsearch/plugins -v edata:/var/lib/elasticsearch --name elk sebp/elk:662
```

### redis

```shell
docker run --name redis -d -p 6379:6379 redis 
```

## Docker Compose

docker容器的的批处理文件，可以通过一个yml文件定义多个容器的docker应用。通过yml文件管理多个docker。docker-compose包含3个概念：services、Networks、Volumes。

### Services

一个service代表以container。

### Volumes

映射docker的数据卷

### networks

容器之间的链接

### 安装

```shell
# 下载docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# 修改权限
sudo chmod +x /usr/local/bin/docker-compose
```

### 使用

* `docker-compose`中很多命令与docker类似

```shell
docker-compose --version # 查看版本

docker-compose up # 启动默认dockercompose
docker-compose up -d # 启动并后台执行
docker-compose -f [文件名] up # 从指定文件启动dockercompose
docker-compose ps # 查看当前所有服务
docker-compose stop # 停止
docker-compose down # 停止删除
```

### yml文件

```yml
version: '3' # 版本
services: # 服务
	# django服务
  web:
    image: registry.cn-beijing.aliyuncs.com/hughxusu/ubuntu_anaconda:superme
    volumes:
      - ./apps:/apps
    command: /opt/anaconda3/envs/superme/bin/uwsgi -i /apps/superme/uwsgi.ini
	# nginx服务
  nginx:
    image: nginx
    ports:
      - "30080:80"
    volumes:
      - ./apps/superme/static:/usr/share/nginx/html/static
      - ./nginx:/etc/nginx/conf.d/
    links: # 应dns域名链接网络
      - web
    depends_on:
      - web
    restart: always
```

