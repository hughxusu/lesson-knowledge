# Nginx

## Docker Nginx 使用



## 基础

### 安装目录

| 路径                                                         | 类型     | 作用                                       |
| ------------------------------------------------------------ | -------- | ------------------------------------------ |
| /etc/logrotate.d/nginx                                       | 配置文件 | Nginx日志轮转，用于logrotate服务的日志切割 |
| /etc/nginx                                                   | 安装目录 | nginx安装目录                              |
| /etc/nginx/nginx.conf                                        | 配置文件 | 用于配置文件                               |
| /etc/nginx/conf.d/default.conf                               | 配置文件 | 默认配置文件                               |
| /etc/nginx/fastcgi_params<br />/etc/nginx/uwcgi_params<br />/etc/nginx/scgi_params | 配置文件 | cgi配置相关，fastcgi配置                   |
| /etc/nginx/koi-utf<br />/etc/nginx/koi-win<br />/etc/nginx/win-utf | 配置文件 | 编码转换映射转化文件                       |
| /etc/nginx/mine.types                                        | 配置文件 | 设置http协议的Content-Type与扩展名对应关系 |
| /lib/systemd/system/nginx-debug.service<br />/lib/systemd/system/nginx.service | 配置文件 | 用于配置出系统守护进程管理器管理方式       |
| /etc/nginx/modules                                           | 安装目录 | nginx模块目录                              |
| /var/cache/nginx                                             | 目录     | 缓存目录                                   |
| /var/log/nginx                                               | 目录     | nginx日志目录                              |
| /usr/sbin/nginx<br />/usr/sbin/nginx-debug                   | 目录     | nginx命令工具                              |

### 安装编译参数

```shell
nginx -V # 版本信息，即相关安装编译参数
```

| 编译选项                                                     | 作用                                          |
| ------------------------------------------------------------ | --------------------------------------------- |
| --prefix=/etc/nginx <br/>--sbin-path=/usr/sbin/nginx<br/>--modules-path=/usr/lib/nginx/modules<br/>--conf-path=/etc/nginx/nginx.conf <br/>--error-log-path=/var/log/nginx/error.log <br/>--http-log-path=/var/log/nginx/access.log<br/> --lock-path=/var/run/nginx.lock | 安装目录或路径                                |
| --pid-path=/var/run/nginx.pid                                | 启动pid的文件                                 |
| --http-client-body-temp-path=/var/cache/nginx/client_temp<br/>--http-proxy-temp-path=/var/cache/nginx/proxy_temp<br/>--http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp<br/>--http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp<br/>--http-scgi-temp-path=/var/cache/nginx/scgi_temp | 执行对应模块时，<br />Nginx所保留的临时性文件 |
| --user=nginx <br />--group=nginx                             | nginx进程启动的用户和用户组                   |
| --with-cc-opt=[para]                                         | 设置额外参数将被添加到，<br />CFLAGS变量      |
| --with-ld-opt=[para]                                         | 设置附件的参数，链接系统库                    |

### Nginx默认配置语法

| 参数           | 含义                        |
| -------------- | --------------------------- |
| user           | 设置nginx服务的系统使用用户 |
| worker_process | 工作进程数(和cpu个数相同)   |
| error_log      | nginx错误日志               |
| pid            | nginx服务启动的pid          |

#### `event`模块

| 参数               | 含义                                        |
| ------------------ | ------------------------------------------- |
| worker_connections | 每个进程允许的最大链接数(一般是10000个作用) |
| use                | 内核模型使用默认设置                        |

