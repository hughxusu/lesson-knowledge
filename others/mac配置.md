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

### 安装

[brew安装介绍](https://brew.sh/index_zh-cn)

```shell
# 打开，保存为brew_install
https://raw.githubusercontent.com/Homebrew/install/master/install.sh

# 替换
BREW_REPO="https://github.com/Homebrew/brew"
# 为
BREW_REPO="https://mirrors.ustc.edu.cn/brew.git"

# 执行
/bin/bash brew_install

# 进入clone状态后停止，进入文件夹
cd /usr/local/Homebrew/Library/Taps/homebrew
git clone https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git # 重新clone
```

### 镜像替换

homebrew主要分两部分：git repo（位于GitHub）和二进制bottles（位于bintray）。

#### 默认源替换

替换homebrew默认源

```shell
# 替换brew.git:
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

# 替换homebrew-core.git:
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
```

替换Homebrew Bottles默认源

```shell
# bash用户
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile

# zsh用户
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```

#### 重置默认源

重置homebrew默认源

```shell
# 重置brew.git:
cd "$(brew --repo)"
git remote set-url origin https://github.com/Homebrew/brew.git

# 重置homebrew-core.git:
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core.git
```

重置Homebrew Bottles默认源，==注释bash或zsh中的命令==

### 常用命令

```shell
# 安装软件
brew install git

# 卸载软件
brew uninstall git

# 查询软件，/gi*/是个正在表达式。
brew search /gi*/

# 简洁命令帮助
brew —help

# 完整命令帮助       
man brew

# 安装软件包(这里是示例安装的Git版本控制)           
brew install git

# 卸载软件包
brew uninstall git

# 搜索软件包
brew search git

# 显示已经安装的所有软件包
brew list

# 同步远程最新更新情况，对本机已经安装并有更新的软件用*标明    
brew update

# 查看已安装的哪些软件包需要更新
brew outdated

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

### 常用路径和文件夹

```shell
/usr/local/Cellar      # 所有brew安装的程序，都将以[程序名/版本号]存放于本目录下
/usr/local/Homebrew    # brew程序自身命令集

# 部分程序的软连接
/usr/bin
/usr/sbin

/usr/local/include # 部分程序的头文件
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
