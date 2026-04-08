# 其它命令

## 安装软件

在Linux系统中安装、更新和卸载软件可以通过`apt`（Advanced Packaging Tool）命令。

1. `install`参数安装软件

```shell
sudo apt install sl
```

2. `upgrade`参数更新已安装的包

```shell
sudo apt upgrade sl
```

3. `remove`参数卸载软件包

```shell
sudo apt remove sl
```

### 配置软件源

Ubuntu中`apt`命令下载软件服务器一般位于国外，为了提高软件下载的速度，可以设置软件服务器的镜像源，阿里云ECS的镜像源一般已经设置好了。可以通过`/etc/apt/sources.list`文件查看

```shell
cat /etc/apt/sources.list
```

## 查询系统信息

### 查看时间和日期

1. 查看系统时间

```shell
date
```

2. 查看日历

```shell
cal -y
```

* `-y`选项可以查看一年的日历。

### 磁盘信息

1. 显示磁盘剩余空间，`-h`以人性化的方式显示文件大小。

```shell
df -h
```

2. 显示目录下的文件大小

```shell
du -h
```

### 进程信息

进程是操作系统中正在执行的程序

1. 查看进程的详细状况

```shell
ps aux 
```

* `a`显示终端上的所有进程，包括其他用户的进程。
* `u`显示进程的详细状态。
* `x`显示没有控制终端的进程。