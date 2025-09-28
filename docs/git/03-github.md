# 开源软件与Github

##  什么是开源

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/git/Open-Source-vs.-Closed-Source.jpg" style="zoom: 55%;" />

开源软件：开放源代码

* 源代码是公开的
* 任何人都可以去查看，修改和使用开源代码

闭源软件：软件的代码是封闭的

* 只有作者能看到闭源软件的代码
* 只有作者能对源代码进行修改

### 开源许可协议

开源并不意味着完全没有限制，为了限制使用者的使用范围和保护作者的权利，每个开源项目都应该遵守开源许可协议（ Open Source License ）。

常见的 5 种开源许可协议：

* BSD（Berkeley Software Distribution）
* Apache Licence 2.0
* GPL（GNU General Public License）
  * 具有传染性的一种开源协议，不允许修改后和衍生的代码做为闭源的商业软件发布和销售
  * 使用 GPL 的最著名的软件项目是：Linux
* LGPL（GNU Lesser General Public License）
* MIT（Massachusetts Institute of Technology, MIT）
  * 是目前限制最少的协议，唯一的条件：在修改后的代码或者发行包中，必须包含原作者的许可信息
  * 使用 MIT 的软件项目有：jquery、Node.js

[各种开源协议介绍](https://www.runoob.com/w3cnote/open-source-license.html)

### 开源软件的优势

1. 开源给使用者更多的控制权。
2. 开源让学习变得容易。
3. 开源才有真正的安全。

开源使软使在每个开发者的工作都可以在前任的基础上进行，不用自己重复造轮子，让开发越来越容易。

### 开源项目托管平台

专门用于免费存放开源项目源代码的网站，叫做开源项目托管平台。目前世界上比较出名的开源项目托管平台主要有以下 3 个：

*  [Github](https://github.com/)全球最大的开源项目托管平台，没有之一
* [Gitlab](https://gitlab.cn/)针对企业用户的代码托管平台
* [Gitee](https://gitee.com/)国产的开源项目托管平台

> [!warning]
>
> 以上3个开源项目托管平台，只能托管以Git管理的项目源代码。

## Github

Github是全球最大的开源项目托管平台。因为只支持Git作为唯一的版本控制工具，故名GitHub。

* 关注自己喜欢的开源项目
* 为自己喜欢的开源项目做贡献（Pull Request）
* 和开源项目的作者讨论Bug和提需求（Issues）
* 把喜欢的项目复制一份作为自己的项目进行修改（Fork）
* 创建属于自己的开源项目

### 注册账号

在Github首页点击`Sign up`按钮，进入注册页面，根据提示完成注册。

* 注册需要时需要用户名、邮箱、密码。
* 注册成功后需要再邮箱内激活账号。

### 远程仓库

#### 创建远程仓库

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/git/Xnip2024-11-13_14-31-46.jpg" style="zoom:35%;" />

> [!warning]
>
> 仓库的名字使用英文，空格用`_`或`-`代替。

#### 远程仓库访问的两种方式

Github上的远程仓库，有两种访问方式，分别是HTTPS和SSH。

* HTTPS：零配置。但是每次访问仓库时，需要重复输入Github的账号和密码才能访问。
* SSH：需要进行额外的配置。配置成功后，每次访问仓库时，不需重复输入Github的账号和密码。

> [!warning]
>
> 在实际开发中，推荐使用 SSH 的方式访问远程仓库。

#### 基于HTTPS上传本地仓库

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/git/Xnip2024-11-13_14-55-10.jpg" style="zoom:35%;" />

将代码上传到远程仓库

```shell
# 首次上传代码
git push -u origin main

# 上传仓库更新后的代码
git push
```

### SSH key 使用

SSH key（SSH密钥）是一种用于安全访问远程计算机系统的加密密钥对。

* 用于远程服务器登录、代码托管平台访问和自动化脚本和工具集成。
* 使用SSH key可以实现免登录身份认证、数据加密传输。
* SSH key 由两部分组成：
  * `id_rsa`（私钥文件，存放于客户端的电脑中即可）
  * `id_rsa.pub`（公钥文件，需要配置到Github中）

#### 生成SSH key

打开终端（Windows打开Git bash），输入如下命令

```shell
ssh-keygen -t rsa -b 4096 -C "xxx@example.com"
```

连续回车，确认生成加密文件，文件一般存放在`用户\.ssh\`文件夹下。

#### 在Github上配置SSH key

1. 打开`.ssh\`文件夹下` id_rsa.pub`文件，复制里面的文本内容。
2. 在Github网站中，`用户->Settings->SSH and GPG Keys->New SSH key`。
3. 将`id_rsa.pub`文件中的内容，粘贴到Key对应的文本框中。
4. 在Title文本框中任意填写一个名称，来标识这个Key。

测试SSH key是否配置成功

```shell
ssh -T git@github.com
```

#### 基于SSH方式上传仓库

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/git/Xnip2024-11-13_15-45-52.jpg" style="zoom:35%;" />

#### 将远程仓库代码复制到本地

在Github中找到要复制的仓库

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/git/Xnip2024-11-13_16-03-53.jpg" style="zoom:35%;" />

在要保存的目录下打开终端（Windows打开Git bash）输入命令

```shell
git clone 仓库地址
```

