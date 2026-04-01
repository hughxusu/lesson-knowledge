# Mac系统常用配置和使用

## 常用概念

### 环境变量

环境变量，系统中使用的变量，键值对形式，每个变量名对应一个或多个值。

#### `PATH`环境变量

在命令行窗口打开一个文件或调用一个程序时，系统首先在当前目录下查找文件或程序，如果找到则打开；如果没有找到则会依次到环境变量path的路径中查找，直到找到为止；最终没找到则报错。

## Mac配置信息

```shell
# Mac系统的环境变量，加载顺序为：
/etc/profile
/etc/paths 
~/.bash_profile 
~/.bash_login 
~/.profile 
~/.bashrc
```

## Homebrew安装及常用命令

Homebrew 是 macOS 上最流行的包管理器，可以方便地安装和管理各种命令行工具。

### 安装

[brew安装](https://brew.sh/zh-cn/)，brew程序包含三部分：

* Brew包管理系统的核心程序（命令行工具）。
* Homebrew-core官方核心软件仓库，指定了软件下载的路径和依赖。
* Bottles提前编译好的二进制软件包。

使用brew安装软件后，软件的安装路径（apple芯片）

* brew程序自身命令集：`/opt/homebrew`。
* brew下载安装的程序：`/opt/homebrew/Cellar`

### 镜像替换

brew默认从国外服务器下载软件，查看brew的镜像源

```shell
cd "$(brew --repo)" && git remote -v
```

将brew镜像源替换为中科大镜像源可以加快软件下载

1. 替换brew仓库源

```shell
git -C "$(brew --repo)" remote set-url origin https://mirrors.ustc.edu.cn/brew.git
```

2. homebrew 4.0后homebrew-core仓库源不需要单独配置。

3. 替换bottles镜像

```shell
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc

source ~/.zshrc

# bash用户
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile

source ~/.bash_profile
```

4. 重置homebrew默认源

```shell
# 恢复 brew 仓库为官方源
git -C "$(brew --repo)" remote set-url origin https://github.com/Homebrew/brew.git

# 恢复bottles移除.zshrc或.bash_profile中的命令
```

### 常用命令

```shell
# 安装软件
brew install git

# 卸载软件
brew uninstall git

# 查询软件，/gi*/是正则表达式。
brew search /gi*/

# 简洁命令帮助
brew —help

# 完整命令帮助       
man brew

# 显示已经安装的所有软件包
brew list

# 更新brew软件   
brew update

# 查看已安装的哪些软件包需要更新
brew outdated

# 更新全部安装包
brew upgrade

# 更新单个软件包
brew upgrade git

# 查看软件包信息  
brew info git

# 访问软件包官方站  
brew home git

# 清理所有已安装软件包的历史老版本
brew cleanup

# 清理单个已安装软件包的历史版本
brew cleanup git   
```

## mac git升级

1. 使用`brew`安装最新`git`

```shell
brew install git
```

2. 改变默认 `git` 指向

在终端中查看我们的 `git` 指向和版本信息

```shell
which git
/usr/bin/git
git --version
git version 2.17.2 (Apple Git-113)
```

通过 `brew link` 将 `git` 指向我们通过 `Homebrew` 安装的 `git`

```shell
brew link git --overwrite
Warning: Already linked: /usr/local/Cellar/git/2.20.1
To relink: brew unlink git && brew link git
```

link 成功后，退出终端后，再次打开。然后查看 `git` 指向和版本信息。

```shell
which git
/usr/local/bin/git
git --version
git version 2.20.1
```

## java安装

* 下载java安装包
* 安装测试

```shell
java -version
javac
```

## bash常用配置

```shell
export JAVA_HOME=$(/usr/libexec/java_home)
export ANDROID_HOME=/Users/hughxusu/Library/Android/sdk
export GOPATH=/Users/hughxusu/go
export PATH=$GOPATH:$JAVA_HOME/bin:$ANDROID_HOME/tools:$ANDROID_HOME/platform_tools:$PATH
export CLASS_PATH=$JAVA_HOME/lib
```

### 使用zsh替换bash

```shell
chsh -s /bin/zsh # 切换为zsh
# 在home目录下增加.zshrc文件，并向.bash_profile拷贝到文件中

# 去掉安全提示
sudo chown -R root:staff /usr/local/share/zsh
sudo chmod -R 755 /usr/local/share/zsh
```

## 常见问题

### 系统升级git无效

```bash
# xcode-select: error: command line tools are already installed, use "Software Update" to install
sudo rm -rf /Library/Developer/CommandLineTools

sudo xcode-select --install
sudo xcode-select -switch /
```
