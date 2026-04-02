# Docker安装与配置

[Docker](https://www.docker.com/)是一个能将应用程序及其依赖环境打包在一起，实现快速、一致部署的容器化平台。

## Docker的安装

### Mac安装

[Docker软件下载](https://www.docker.com/products/docker-desktop/)

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/docker/Xnip2025-12-16_13-58-11.jpg" style="zoom:85%;" />

安装成功后启动Docker程序

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/docker/Xnip2025-12-16_14-15-36.jpg" style="zoom:85%;" />

设置服务器镜像

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/docker/Xnip2025-12-16_14-23-06.jpg" style="zoom:85%;" />

在引擎设置中添加如下内容

```json
{
  "builder": {
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  },
  "experimental": false,
  "registry-mirrors": [
    "https://hub-mirror.c.163.com",
    "https://docker.1ms.run",
    "https://docker.xuanyuan.me"
  ]
}
```

> [!warning]
>
> 设置完成后需要退出Docker软件，重新启动。

重启后在终端中查看配置是否生效

```shell
docker info
```

终端中显示镜像信息如下

```shell
 Registry Mirrors:
  https://hub-mirror.c.163.com/
  https://docker.1ms.run/
  https://docker.xuanyuan.me/
```

表示镜像配置成功。

## Ubuntu的安装
