

# 用户与权限管理

在Linux系统中，不论是由本机或是远程登录，都必须有一个账号，并且对于不同的系统资源拥有不同的使用权限，系统资源包括：文件和目录、网络资源（端口访问）、安装和执行软件等。用户权限包括

| 权限 |  英文  | 缩写 | 数字代号 |
| :--: | :----: | :--: | :------: |
|  读  |  read  |  r   |    4     |
|  写  | write  |  w   |    2     |
| 执行 | excute |  x   |    1     |

用户组（User Group）是一种将多个用户集合在一起，以便统一分配权限的逻辑机制。

1. 预先针对用户组设置好权限。
2. 将不同的用户添加到对应的组中。
3. 不用依次为每一个用户设置权限。

用户和用户组的关系

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/2144544-20201012203745705-413629468.png" style="zoom:65%;" />

## 权限管理

使用`ls -l`可以查看文件夹下文件的详细信息

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-06_11-57-01.jpg" style="zoom:65%;" />

* 权限：操作文件或文件夹的权限，包括：读、写和执行。
* 硬链接数：有多少种方式，可以访问到当前目录。
* 拥有者：哪个用户创建了文件或目录。
* 用户组：文件属于哪个组。

> [!warning]
>
> 文件的拥有者可以不属于该文件的所属组。

权限信息表示

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/002_权限示意图.png" style="zoom:80%;" />

### 修改文件权限

使用`chmod`可以修改用户对文件或目录的权限

```shell
chmod [+|-]rwx [文件名|目录名]
```

* `+`表示添加权重，`-`表示删除权限。
* `rwx`表示修改的权限。

1. 修改文件可执行权限

```shell
chmod +x hello.sh
```

2. 移除文件可读权限

```shell
chmod -r hello.sh 
```

3. 修改目录可执行权限，如果文件夹没有可以执行权限，就无法进入。

```shell
chmod -x codes
```

4. 移除目录的读写权限，目录可以访问，但无法查看文件，添加文件。

```shell
chmod -rw codes
```

修改权限的数字形式

```shell
chmod -R 777 codes/
```

* `-R`递归子目录或文件。
* `777`第一个7表示拥有者权限，第二个7表示组权限，第三个7表示其他用户权限。

![](https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/004_文件权限示意图.png)

> [!warning]
>
> 使用数组表示可以同时修改拥有者、组和其他用户三者权限。

### 超级用户

Linux中的超级管理员用户称为root用，root用户对于操作系统的所有资源具有所有操作权限。除root用户外的其它用户，称为标准用户。标准用户执行系统管理相关命令，如：添加用户、安装软件需要借助`sudo`命令。

`sudo`允许标准用户执行系统操作命令

* `sudo`操作需要输入用户自己的密码，之后有5分钟的有效期限，超过期限则必须重新输入。
* 所有`sudo`执行的命令都会被记录在日志中，方便日后追溯。
* 若其未经授权的用户企图使用`sudo`，可以发出警告邮件给管理员。

## 用户组管理

组管理的操作属于系统操作，需要使用`sudo`命令

`groupadd`命令可以添加组

```shell
#     命令     组名
sudo groupadd dev
```

查看创建的组信息

```shell
cat /etc/group
```

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-06_20-13-32.jpg" style="zoom:55%;" />

> [!warning]
>
> `/etc`文件夹中保存配置相关文件，包括：组信息、用户信息和密码信息等。

使用`chgrp`命令可以修改文件或目录的所属组

```shell
#    命令      组  文件或目录
sudo chgrp -R dev codes/
```

`groupdel`命令可以删除组

```shell
#    命令      组
sudo groupdel dev
```

用户创建的默认组，并没有执行系统管理相关命令的权限，需要在`/etc/sudoers`中配置相关权限。

```shell
sudo cat /etc/sudoers
```

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-07_19-49-07.jpg" style="zoom:55%;" />

* 系统默认存在的`admin`组和`sudo`组与`root`用户一样有所有权限。

> [!warning]
>
> 没有在这个文件里配置的用户和用户组，没有系统管理相关命令的权限。

## 用户管理

### 用户基本操作

创建用户

1. `useradd`命令用于添加用户

```shell
sudo useradd -m -g dev dev-one
```

* `-m`自动建立用户家目录。
* `-g`指定用户所在组，`-g dev`用户组为`dev`。
* `dev-one`新建的用户名。

2. `passwd`命令用于设置用户的密码

```shell
#    命令    用户名
sudo passwd dev-one
```

查看用户信息，`/etc/passwd`保存了所以用户信息

```shell
cat /etc/passwd
```

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-07_21-05-52.jpg" style="zoom:60%;" />

`userdel`命令用于删除用户

```shell
sudo userdel -r dev-one
```

* `-r`自动删除用户家目录

> [!warning]
>
> 创建用户时，如果忘记添加`-m`选项，指定新用户的家目录，最好的方法就是删除用户，重新创建。

切换用户

1. `su`命令可以在同一终端内，切换不同的用户。

```shell
su - root
```

* `-`切换到`root`家目录，否则保持位置不变。

2. `exit`退出当前的切换用户。

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/003_su和exit示意图.png" style="zoom:90%;" />

### 查看用信息

1. 查看用户ID和组ID

```shell
id dev-one
```

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-07_22-03-50.jpg" style="zoom:65%;" />

2. `who`查看当前所有登录的用户终端。
3. `whoami`查看当前登录用户的账户名。

### 修改用户权限

使用`usermod`可以修改用户组和Shell程序

1. `-G`修改用户的附加组

```shell
#               组名称               
sudo usermod -G sudo dev-one
```

2. `-g`修改用的主组

```shell
#               组名称             
sudo usermod -g sudo dev-one
```

3. `-s`修改用户的Shell程序

```shell
#               修改bash
sudo usermod -s /bin/bash dev-one
```

### 查看程序位置

`which`命令可以查看可执行程序的位置，Linux操作系统中的部分命令就是可执行程序。

```shell
which passwd
```

* 这里的`passwd`是一个可执行程序，保存在`/usr/bin/passwd`路径下。
* `/etc/passwd`中的`passwd`是一个文本文件，用于保存用户信息。

Linux的命令分为外部命令和内部命令两类：

1. 外部命令，对应着磁盘上的一个可执行文件，如：`ls`、`cat`等。
2. 内部命令，没有独立的可执行文件，它们是Shell程序自带的功能，如：`cd`、`pwd`等。

```shell
which cd
```

程序存储路径

```shell
.
├── bin -> usr/bin        # 软连接指向bin
├── sbin -> usr/sbin      # 软连接指向sbin
└── usr
     ├── bin              # 二进制执行文件目录
     └── sbin             # 二进制代码存放目录
```

### 修改文件权限

`chown`修改文件和目录的拥有者

```shell
#    命令   递归子目录或文件   用户名   目录
sudo chown -R             dev-one codes
```

`chgrp`修改文件和目录

```shell
#    命令   递归子目录或文件   用户名   目录
sudo chgrp -R              dev    codes
```
