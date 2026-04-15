# 连接服务器

管理和操作服务器，是开发人员的日常工作，一般都给通过ssh客户端完成。

```mermaid
	graph LR;
c(ssh客户端)--互联网-->aa(linux服务器)
```

## 阿里云服务

这里以阿里云的ECS为为例，创建一个Linux云服务器实例。阿里云常用的服务有：

1. ECS服务器：云端的电脑主。
2. 轻量级应用服务器：基于云服务器ECS的底层架构，集成常用功能，帮助开发者快速构建应用程序和网站。
3. RDS数据库：相当于安装了数据库的服务器主机。
4. PolarDB数据库：采用了共享存储架构的数据库服务，使用与RDS没有区别。
5. OSS对象存储服务：用来存储海量的、各种类型的非结构化数据，包括：视频、图片等。

阿里云服务器费用一般包括：

1. 计算资源费：主要是CPU和内存。
2. 存储资源费：这是服务器硬盘的费用。
3. 网络带宽费：服务器与公网通信的费用。有两种计费模式：
   *  按固定带宽计费：类似于包月宽带，分配一个固定的带宽值，在计费周期内可以无限使用流量。
   * 按使用流量计费：类似于手机流量，一个带宽峰值，然后按照实际产生的出站流量来付费。

### 创建VPC网络

在阿里云上创建一个VPC网络，相当于租用一块完全私有、逻辑隔离的虚拟网络空间，VPC网络并不收费。如果云服务（ECS服务器、数据库服务、对象存储OSS等），在同一个VPC网，且处于同一个地域中，可以通过阿里云的内网进行数据传输，不产生公网流量费用。

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-05_10-27-50.jpg" style="zoom:45%;" />

创建专有VPC网络的配置

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-05_10-24-10.jpg" style="zoom:45%;" />

### 创建ECS服务器

选择ECS服务器

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-05_10-42-34.jpg" style="zoom:45%;" />

选择购买的实力

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-05_10-46-06.jpg" style="zoom:45%;" />

配置ECS实例

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-05_10-59-30.jpg" style="zoom:45%;" />

### 服务器配置

选择ECS服务器

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-05_12-01-04.jpg" style="zoom:45%;" />

服务器实例选择

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-05_15-56-07.jpg" style="zoom:45%;" />

## 连接服务器

连接远程服务器主要通过互联网来实现

* 网卡：专门负责网络通讯的硬件设备。
* IP：设置在网卡上的地址信息

网卡类似于手机，IP地址类似于手机号，每台联网的电脑都有IP地址，保证电脑之间正常通讯。

### 检查网络

使用`ifconfig`命令可以查看电脑的网卡配置信息

```shell
ifconfig | grep inet
```

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-05_16-27-01.jpg" style="zoom:45%;" />

> [!warning]
>
> 使用`ifconfig`查询的地址是阿里云服务器的内网IP，无法使用SSH客户端链接。

使用`ping` 一般用于检测当前计算机到目标计算机之间的网络是否通畅。数值越大，速度越慢。

* 测试本地网卡是否畅通

```shell
ping 127.0.0.1
```

* 测试网络连接是否正常

```shell
ping www.baidu.com
```

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-05_20-47-31.jpg" style="zoom:45%;" />

### 域名和端口号

域名就是IP地址的别名，方便用户记忆。端口号：通过端口号可以找到计算机上运行的应用程序。常见服务端口号为

| 服务      | 端口号 |
| :-------- | :----- |
| SSH服务器 | 22     |
| Web服务器 | 80     |
| HTTPS     | 443    |
| FTP服务器 | 21     |

阿里云服务器默认只打开了，SSH服务器端口和Web服务器端口，用户自己部署服务时需要设置规则打开接口

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-15_22-13-59.jpg" style="zoom:45%;" />

给服务器添加访问规则

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-15_23-10-58.jpg" style="zoom:40%;" />

### SSH客户端

SSH客户端是一种使用Secure Shell（SSH）协议连接到远程计算机的软件程序，SSH协议专为远程登录会话和其他网络服务提供安全性的协议。

* 可以有效防止，远程管理过程中的信息泄露。
* 可以对所有传输的数据进行加密。
* 传输的数据是经过压缩的。

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Securing_applications_with_ssh_tunneling___port_forwarding-2.png" style="zoom:45%;" />

Mac电脑上最方面的SSH客户端为[Termius](https://termius.com/)，Termius免费版既可以日常使用，且支持Windows版。

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-05_21-25-14.jpg" style="zoom:60%;" />

创建SSH客户端

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-05_21-36-08.jpg" style="zoom:50%;" />

### 文件传递

有时候需要在和服务器之间上传文件，Termius同样集成了这一功能。

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-05_21-45-41.jpg" style="zoom:55%;" />

选择传递文件的目录

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-09_18-40-14.jpg" style="zoom:60%;" />

本地目录要输入账号名和密码

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-09_18-49-47.jpg" style="zoom:65%;" />

> [!warning]
>
> Mac电脑需要设置`系统设置->通用->共享`中开启远程登录。

文件传送

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/linux/Xnip2026-04-09_18-59-01.jpg" style="zoom:65%;" />
