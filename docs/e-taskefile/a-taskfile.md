# Taskfile

Taskfile是一个现代化的命令行任务运行器，可以把它看作Makefile的替代品。它可以统一管理项目中的常用命令：

* 启动开发服务器
* 运行测试
* 数据库迁移
* 构建前端

## 安装Task工具

[Taskfile的安装指南](https://taskfile.dev/docs/installation)

在Mac电脑上借助Homebrew可以安装Taskfile工具

```shell
brew install go-task
```

安装成功后可以查看Taskfile工具的版本

```shell
task --version
```

Taskfile工具迭代比较迅速，升级Taskfile的版本

```shell
brew upgrade go-task
```

## 初始化项目

对应没有使用Taskfile管理的项目可以使用，如下命令初始化

```shell
task --init
```

初始化之后根目录下出现一个`Taskfile.yml`文件，该文件用于配置Task命令。Taskfile同时还接受如下文件名

```shell
taskfile.yml
Taskfile.yaml
taskfile.yaml
```

初始化后`Taskfile.yml`中包含如下内容

```yaml
# yaml-language-server: $schema=https://taskfile.dev/schema.json

version: '3'

vars:
  GREETING: Hello, world!

tasks:
  default:
    desc: Print a greeting message
    cmds:
      - echo "{{.GREETING}}"
    silent: true
```

* `version`、`vars`和`tasks`顶级声明，属于Taskfile的核心配置，有Taskfile规范预定义。

Taskfile常见的顶级申明

| 声明        | 作用                   | 简要说明                                                     |
| :---------- | :--------------------- | :----------------------------------------------------------- |
| `version`   | 指定Taskfile的语法版本 | 必填项。当前的版本为`'3'`。用于确保`task`命令能以兼容的方式解析文件 |
| `includes`  | 引入其他Taskfile 文件  | 用于将任务拆分到多个文件中，实现模块化管理。                 |
| `vars`      | 定义全局变量           | 用于定义在文件任何地方都可使用的变量，相当于编程中的定义常量。 |
| **`tasks`** | 定义执行命令           | 包含了所有需要执行的任务定义。                               |
| `env`       | 设置全局环境变量       | 为所有任务的命令执行环境设置变量。                           |
| `dotenv`    | 加载`.env`文件         | 指定一个或多个`.env`文件的路径，将其中定义的环境变量加载到任务执行环境中。 |
| `run`       | 设置任务默认执行策略   | 定义当同一任务在一次运行中被多次调用时的行为。               |

`var`的定义

```yaml
vars:
  GREETING: Hello, world!
```

* 定义了一个名为`GREETING`的变量，值为`"Hello, world!"`。

`tasks`用于定义一个个具体的自动化任务。

```yaml
tasks:
  default:                # 
    desc: Print a greeting message
    cmds:
      - echo "{{.GREETING}}"  # 执行 echo 命令，并引用上面的 GREETING 变量
    silent: true          # 静默模式，不打印 echo 命令本身，只输出结果
```

* 任务名称为`"default"`，`default`是一个特殊任务名，直接在终端输入`task`就会执行它。
* 常用属性
  * `desc`：任务的简短描述，运行`task --list`时会显示。
  * `cmds`：任务要执行的实际Shell命令列表。
    * `- echo "{{.GREETING}}"`每个一行都代表一个独立的Shell命令。
  * `silent`：是否静默执行，不打印命令本身。
