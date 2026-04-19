# 容器编排与镜像制作

## Docker Compose

Docker Compose是一个用操作运行多容器的工具，它通过一个`compose.yaml` 来配置配置容器的服务、网络和卷。Mac和阿里云安装Docker的过程中已经默认安装的Docker Compose。

* 可以通过`compose.yaml` 启动和停止多个容器。
* 可以通过`compose.yaml`启动和停止一部分容器。
* 可以多部分应用扩容。
* 可以统一清理容器和数据卷。

查看Docker Compose的版本

```shell
docker compose version  # 旧版本的命令docker-compose --version
```

* [Docker Compose使用](https://docs.docker.com/compose/)
* [`compose.yaml`说明](https://docs.docker.com/reference/compose-file/)

Docker Compose中常用的顶级元素

* `include`V2.20+新增允许引入其他的Compose文件，实现配置的模块化拆分。
* `name`定义项目的名称。
* `services`定义容器的镜像、环境变量、网络、卷和启动逻辑。
* `networks`需要创建的网络。
* `volumes`需要创建的卷名。
* `configs`以非敏感方式将配置文件注入到容器中。
* `secrets`专门用于处理敏感信息。

以Wordpress为例创建`compose.yaml`文件

```yaml
name: wordpress
services:
  # 定义一个容器
  mysql:
    # 容器名称，可以省略，省略后与容器定义标识一致
    container_name: mysql
    
    # 创建容器使用的镜像
    image: mysql:8.0 
    
    # 端口号的映射
    ports:
      - 3306:3306
      
    # 容器环境变量设置，相当于docker run的-e参数 
    environment:
      - MYSQL_ROOT_PASSWORD=123456
      - MYSQL_DATABASE=wordpress

    # 容器卷映射，相当于docker run的-v参数 
    volumes:
      # 卷映射需要在后面的volumes顶级元素中，再次声明
      - mysql-data:/var/lib/mysql 
      # 挂载文件夹不需要在volumes中声明
      - /opt/docker/mysql/conf:/etc/mysql/conf.d
      - /opt/docker/mysql/logs:/var/log/mysql
      
    # 容器重启后项目自动重启
    restart: always
    
    # 加入网络
    networks:
      - blog

  wordpress:
    image: wordpress
    ports:
      - 80:80
      
    # 容器环境变量设置，也可以使用键值对形式设置
    environment:
      WORDPRESS_DB_HOST: mysql
      WORDPRESS_DB_USER: root
      WORDPRESS_DB_PASSWORD: 123456
      WORDPRESS_DB_NAME: wordpress

    volumes:
      - wordpress-data:/var/www/html
    networks:
      - blog
      
    # 控制服务的启动与停止顺序
    depends_on:
      - mysql

# 声明容器的卷映射
volumes:
  mysql-data:
  wordpress-data:

# 声明容器的依赖网
networks:
  # 网络名称
  blog:
    driver: bridge # 网络网络类型，可以省略，默认为桥接
```

* `/opt`是Linux专门留给安装第三方应用程序的目录，适合存放项目的配置文件。

启动项目

```shell
docker compose up -d
docker compose -f compose.yaml up -d
```

* `-d`后台启动全部容器。
* `-f compose.yaml`使用哪个`.yaml`文件启动，如果省略直接调用文件夹下的`compose.yaml`。

> [!warning]
>
> 1. 尽量使用Docker Compose来管理容器和网络名称。
> 2. 不同的项目，使用不同的文件夹区分，文件夹下一般只保留一个`compose.yaml`文件