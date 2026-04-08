### 用户管理

### Linux常用命令

* 其他

```shell
history # 查阅最近使用命令
history 10 # 最近10个命令
!5 # 执行历史变化为5的命令
! # 上一个命令
```

* `export`命令

```shell
export PATH=$PATH:/root # 临时加入环境变量
```

* 归档

```shell
# -c 生成档案文件 -v 列出详细过程 -f 制定档案文件名称
# 生成文件 归档文件列表
tar -cvf a.tar a.txt b.txt

# 解包 -x解包文件
tar -xvf a.tar

# -t列出档案中文件的名称
tar -tvf a.tar
```

* 压缩

```shell
# 对文件压缩，生a.tar.gz文件，压缩前文件自动删除
gzip a.tar  

# 解压，解压后解压文件消失
gzi p -d a.tar.gz
```

* 使用断开查询

```shell
netstat -nltp
```

### 环境配置

环境配置文件

```shell
/etc/profile

PATH=$PATH:/home/java/bin # 在原path下追加
export JAVA_HOME # 导出路径
```

### linux分区





```shell
env # 显示环境变量

TERM_PROGRAM=Apple_Terminal
NVM_CD_FLAGS=
SHELL=/bin/bash # 使用的shell
TERM=xterm-256color
TMPDIR=/var/folders/s6/zkshgbrj6x7dw4hp_grqtfrc0000gn/T/
CONDA_SHLVL=1
Apple_PubSub_Socket_Render=/private/tmp/com.apple.launchd.waf7IBe9Od/Render
CONDA_PROMPT_MODIFIER=(base) 
TERM_PROGRAM_VERSION=421.2
OLDPWD=/Users/xusu
TERM_SESSION_ID=6D399074-F203-4F1E-8885-A80F9703ACAE
LC_ALL=en_US.UTF-8
NVM_DIR=/Users/xusu/.nvm
USER=xusu
CONDA_EXE=/Users/xusu/DevelopingKits/anaconda3/bin/conda
SSH_AUTH_SOCK=/private/tmp/com.apple.launchd.jtt3mrR6VP/Listeners
_CE_CONDA=
# 环境变量
PATH=/Users/xusu/.nvm/versions/node/v10.15.3/bin:/Users/xusu/DevelopingKits/anaconda3/bin:/Users/xusu/DevelopingKits/anaconda3/condabin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
CONDA_PREFIX=/Users/xusu/DevelopingKits/anaconda3
PWD=/bin
LANG=en_US.UTF-8
XPC_FLAGS=0x0
_CE_M=
XPC_SERVICE_NAME=0
SHLVL=1
HOME=/Users/xusu
CONDA_PYTHON_EXE=/Users/xusu/DevelopingKits/anaconda3/bin/python
LOGNAME=xusu
NVM_BIN=/Users/xusu/.nvm/versions/node/v10.15.3/bin
CONDA_DEFAULT_ENV=base
_=/usr/bin/env
```



### Vim

* 从命令模式进入编辑模式：`i`插入 / `a`追加
* 命令模式下
  * 保存：`w`   +  [文件名] 保存
  * 删除：
    * 删除一行：`dd`
    * 删除一个单词：`dw`
  * 拷贝：
    * 拷贝一行数据：`yy`
    * 拷贝一个单词：`yw`
  * 将缓冲区文件写入硬盘：`w`+回车
  * 粘贴：`p`
  * 撤销：`u`
  * 光标跳跃：
    * 跳到文件头：`gg`
    * 到最后一行：`g`
    * 跳到行首：shift+`^`
    * 跳到行位：shift+`$`
    * 单词移动：向前`w/2w`，向后`b/2b`
  * 查看行号：`: set nu`
  * 到制定行：行号+`g`
  * 查找：`/`+查找内容
  * 删除光标位置字符：`x`
  * 分窗口：横向`split`/纵向`vsplit`
  * 窗口间跳转：control+`ww`
  * 关闭窗口：`close`
