# 定义数据库

SQL通用语法

1. SQL语句可以单行或多行书写，以分号结尾。
2. SQL语句可以使用空格/缩进来增强语句的可读性。
3. MySQL数据库的SQL语句不区分大小写，通常使用小写。
4. 注释：
   * 单行注释：`--`注释内容或`#`注释内容。
   * 多行注释：`/* 注释内容 */`

## 数据库操作

查询所有数据库

```sql
show databases;
```

在终端中输入

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-16_20-02-02.jpg" style="zoom:45%;" />

查询当前数据库

```sql
select database();
```

查看版本数据库版本

```sql
select version();
```

### 创建数据库

使用`create`命令创建数据库

```sql
create database sqllesson;
```

> [!alert]
>
> 在同一个服务器中，不能创建两个名称相同的数据库，否则将会报错。

```sql
create database sqllesson if not extists sqllesson;
```

创建数据库并指定字符集

```sql
create database lessons default charset utf8mb4;
```

* `charset utf8mb4`使用utf8字符集。

### 删除数据库

使用`drop`命令删除数据库

```sql
drop database if exists lessons;
```

* `if exists`如果数据库存在执行删除，否则不执行删除。

### 切换数据库

在同一个服务器中可以创建多个数据库，使用`use`命令选中需要使用的数据库

```sql
use sqllesson;
```

选择后使用`select database();`查看数据库选择状态。

## 数据库IDE

在控制台中操作数据库十分不便，在日常的开发中，通常会借助图形化界面，操作数据库。

[DataGrip](https://www.jetbrains.com/zh-cn/datagrip/)是[jetbrains](https://www.jetbrains.com/zh-cn/)旗下的一款，适用于关系型和NoSQL数据库的强大跨平台IDE。虽然DataGrip是收费软件学生和教师可以通过[大学电子邮件地址](https://www.jetbrains.com/shop/eform/students)申请免费使用。

> [!warning]
>
> 邮箱必须是以`.edu`结尾的电子邮箱。

[DataGrip软件下载](https://www.jetbrains.com/zh-cn/datagrip/download/)

初始化项目

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-16_22-01-33.jpg" style="zoom:40%;" />

创建新数据库链接

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-16_22-05-56.jpg" style="zoom:40%;" />

配置链接信息

<img src="../../images/mysql/Xnip2025-12-18_13-29-32.jpg" style="zoom:80%;" />

选择数据库

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-16_22-30-08.jpg" style="zoom:40%;" />

使用DataGrip创建数据库

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-16_22-33-55.jpg" style="zoom:40%;" />

架构（Schema）：数据库的“蓝图”，定义了表的结构，以及表之间的关系，等价于Database。使用如下命令也可以创建数据库

```sql
create schema lessons;
```

SQL命令行

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-16_22-41-49.jpg" style="zoom:40%;" />

选中数据库

```sql
use sqllesson;
select database();
```

