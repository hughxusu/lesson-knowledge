# 概念与安装

## 认识数据库

数据库体系包含三个基本概念：

1. 数据库（DataBase，简称DB）：存储数据的仓库，数据是有组织的进行存储。
2. 数据库管理系统（DataBase Management System，简称DBMS）：操纵和管理数据库的大型软件。
3. 结构化查询语言（Structured QueryLanguage，简称SQL）：操作关系型数据库的编程语言，定义了一套操作**关系型数据库**统一标准。

![](https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Database-Management-System.jpg)

[主流数据库管理系统的市场占有率排名](https://db-engines.com/en/ranking)

### MySQL数据库

本教程以[MySQL](https://www.mysql.com/)数据为例，来介绍关系型数据库的使用和操作。MySQL为开源免费的中小型数据库，Sun公司收购了MySQL，而Oracle又收购了Sun公司。官方提供了两种不同的版本

* 社区版（MySQL Community Server）：免费，不提供任何技术支持。
* 商业版（MySQL Enterprise Edition）：收费，官方提供技术支持。

### 关系型数据库

关系型数据库是一种基于“关系模型”（即二维表格模型）来组织和存储数据的数据库。它将数据存储在由行和列组成的表（Table）中，表与表之间可以通过共享的字段（键）建立关系，从而高效地管理和查询结构化数据。

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/6tiZAezIwp-rSNekZQ.tiff" style="zoom:65%;" />

二维表可以理解为类似于Excel一样的表格，每个工作表都是一张表，表头是列，每一行是一条记录。

![](https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/component-of-a-database-table.gif)

## MySQL安装与启动

MySQL网站提供了不同版本的安装程序，包括：Windows、Linux或MacOS。本教程使用[Docker](/docs/03-docker/a-安装.md)来安装MySQL。在Docker Hub中搜索MySQL镜像

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-16_14-30-39.jpg" style="zoom:85%;" />

选择需要的MySQL版本

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-16_14-42-45.jpg" style="zoom:85%;" />

查看镜像软件

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-16_14-45-53.jpg" style="zoom:85%;" />

创建MySQL容器

```shell
docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 -d mysql:8.4.7
```

* `--name mysql`设置容器的名称。
* `-e MYSQL_ROOT_PASSWORD=123456`设置数据库的密码。
* `-p 3306:3306`设置端口号。
* `-d mysql:8.4.7`使用镜像的版本。

查看MySQL容器

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-16_15-07-20.jpg" style="zoom:85%;" />

运行镜像终端

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-16_15-10-15.jpg" style="zoom:85%;" />

在镜像终端中启动MySQL命令行

```shell
mysql -u root -p
```

* `-u root`MySQL数据库用户名。
* `-p`MySQL数据库用户名对应的密码。

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-16_15-19-57.jpg" style="zoom:85%;" />

使用`exit`可以退出MySQL命令行工具。

## SQL

SQL（Structured QueryLanguage）结构化查询语言。操作关系型数据库的编程语言，定义了一套操作关系型数据库统一标准。根据功能，SQL语句分为四类

```mermaid
graph TB
c(SQL)
c-->a(DDL)
c-->b(DML)
c-->e(DQL)
c-->f(DCL)
```

* DDL（Data Definition Language）：数据定义语言，用来定义数据库对象（数据库、表、字段）。
* DML（Data Manipulation Language）：数据操作语言，用来对数据库表中的数据进行增删改。
* DQL（Data Query Language）：数据查询语言，用来查询数据库中表的记录。
* DCL（Data Control Language）：数据控制语言，用来创建数据库用户、控制数据库的访问权限。
