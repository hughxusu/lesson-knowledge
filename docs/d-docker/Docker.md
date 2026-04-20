# Docker安装

## 容器命令

### 基本操作

```shell

docker ps -l # 查看上一个容器
docker ps -n 3 # 查看上3次运行的容器
docker ps -lq # 只显示上一次的容器编号
docker top # 容器内运行的进程

# 启动容器 run 命名
# 交互式启动 -it 参数
docker run -it --name cent_demo centos # --name 进程别名，省略系统自动分配
exit # 退出交互式容器，并结束进程。ctrl+p+q 容器不停止退出

docker start -i b9c025a4d557 # 已交互方式重启容器


# 以ctrl+p+q退出后可以使用
docker attach [容器id] # 进入退出后没有停止的容器
docker exec -t [容器id] ls -l /tmp # 在容器外查询容器内命令
docker exec -it [容器id]  /bin/bash # 进入容器的相应路径

docker stop [容器id] # 停止容器
docker kill [容器id] # 强制停止

docker rm [容器id] # 删除已停止的容器
docker rm -f [容器id] # 强制删除容器
docker rm -f $(docker ps -qa) # 删除所有容器

docker logs [容器id] # 打印容器日志

# 以守护进程方式启动
docker run -d centos # 以后台进行方式启动容器

docker inspect [容器id] # 查看容器内的细节

docker cp [容器id]:/tmp/yum.log ./ # 将容器内的数据拷贝到容器外
docker cp ./index [容器id]:/tmp/ # 将容器外部的文件复制到容器内部

# 宿主机9000端口映射到容器8080端口，启动bash命令行
docker run -it --name myjava -p 9000:8080 java bash 

# 暂停容器
docker pause [容器id]

# 暂停容器继续执行
docker unpause [容器id]

# 从容器生成镜像
docker commit -m '信息' [容器id] [生成image名称]
```

### 全选操作

```shell
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

### nginx

```shell
docker run -it -d --name nginx -v /home/nginx/:/etc/nginx/conf.d -p 30080:80 nginx
```

### redis

```shell
docker run --name redis -d -p 6379:6379 redis 
```







