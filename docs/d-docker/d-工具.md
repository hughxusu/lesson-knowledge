# 容器编排与镜像制作

## `yaml`语法

Yaml是一种文本数据存储方式，类似于Josn文件，Yaml利用缩进来控制数据结构，可读性比Josn更强。

* Yaml是一种所有编程语言可用的、友好的、数据序列化标准。
* Yaml格式像一份“清单”，常用于设置配置文件等信息。

Yaml的基本规则

1. 大小写敏感。
2. 使用缩进表示层级关系。
3. 缩进时不能使用`tab`键，只能使用空格。
4. 缩进的空格数量不重要，只要相同层级的元素左侧对齐即可。
5. Yaml文件的后缀名可以是`.yaml`和`.yml`。

Yaml的数据结构

1. `object`（对象）：键值对的集合。
2. `array`（数组）：一组顺序的序列。
3. `scalars`（纯量）：单个不可拆分的值，包括：字符串、布尔值、整数、浮点数、`null`、日期等。

1. 定义一个简单的`student.yaml`文件

```yaml
# 学生信息
name: tom
age: 18
is_male: true
lesson:
  - math
  - english
  - history

# {
#   "name": "tom",
#   "age": 18,
#   "is_male": true,
#   "lesson": [
#     "math",
#     "english",
#     "history"
#   ]
# }
```

* Yaml文件中可以加入注释，以`#`开始，读取Yaml文件的工具会忽略注释。
* `name: tom`表示一个键值对。
* 数组中的每一项使用`-`表示。

2. 直接定义一个数组

```yaml
- name: tom
  age: 18
  lesson:
    - math
    - english

- name: jane
  age: 19
  lesson:
    - math
    - science

# [
#   {
#     "name": "tom",
#     "age": 18,
#     "lesson": [
#       "math",
#       "english"
#     ]
#   },
#   {
#     "name": "jane",
#     "age": 19,
#     "lesson": [
#       "math",
#       "science"
#     ]
#   }
# ]
```

3. 一个更复杂的文件结构

```shell
employee:
  name: tom
  is_male: true
  birth_date: 1990-01-03 00:00:00
  salary: 100000
  location: ~
  skill:
    - fastapi
    - docker
    - vue
    - sql

  job:
    frontend: vue
    backend: fastapi

  leaders: 
    - group_leader=John
    - project_manager=Jane

# {
#   "employee": {
#     "name": "tom",
#     "is_male": true,
#     "birth_date": "1990-01-03 00:00:00",
#     "salary": 100000,
#     "location": null,
#     "skill": [
#       "fastapi",
#       "docker",
#       "vue",
#       "sql"
#     ],
#     "job": {
#       "frontend": "vue",
#       "backend": "fastapi"
#     },
#     "leaders": [
#       "group_leader": "John",
#       "project_manager": "Jane"
#     ]
#   }
# }
```

* `~`在Yaml文件中表示`null`数据。

## Docker Compose

Docker Compose是一个用操作运行多容器的工具，它通过一个`compose.yaml` 来配置配置容器的服务、网络和卷。Mac和阿里云安装Docker的过程中已经默认安装的Docker Compose。

* 可以通过`compose.yaml` 启动和停止多个容器。
* 可以通过`compose.yaml`启动和停止一部分容器。
* 可以多部分应用扩容。
* 可以统一清理容器和数据卷。

Docker Compose的许多命令与Docker类似，查看Docker Compose的版本

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

### Wordpress部署

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
* 修改`compose.yaml`文件后，使用`docker compose up -d`重启容器，只有修改的容器会重启，其他容器保持不变。

> [!warning]
>
> 1. 尽量使用Docker Compose来管理容器和网络名称。
> 2. 不同的项目，使用不同的文件夹区分，文件夹下一般只保留一个`compose.yaml`文件

停止运行容器

```shell
docker compose stop
```

移除所有容器和相关网络，但是不会移除相关的卷和文件夹

```shell
docker compose down
docker compose down --rmi all -v
```

* `--rmi`移除容器的同时，移除镜像，需要指定镜像名，`all`表示移除所有相关镜像。
* `-v`表示移除所有相关的卷。

> [!warning]
>
> 这里是将容器全部移除，并不是简单的暂停。