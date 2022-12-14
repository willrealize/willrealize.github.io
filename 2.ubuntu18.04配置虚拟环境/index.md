# 使用文档 - Ubuntu18.04配置虚拟环境


ubuntu18.04中安装和使用虚拟环境

<!--more-->

### 1 安装virtualenv

测试有没有安装 `pip3`

> 注意
>
> ubuntu18.04 移除了 python2.7 , 这里使用 pip3
>
> 下面的操作都是在普通用户下

```bash
pip3 list
```

如果提示没有装, 先安装 `pip`

```
sudo apt install python3-pip
```

使用 `pip3` 安装 `virtualenv`

```bash
pip3 install virtualenv -i https://pypi.douban.com/simple
```

> 提示
>
> 这里使用的是豆瓣源, 可以加速下载: `-i https://pypi.douban.com/simple`

### 2 安装virtualenvwrapper

`virtualenvwrapper` 是虚拟环境管理工具

```bash
pip3 install virtualenvwrapper -i https://pypi.douban.com/simple
```

安装完成后还不能使用, 按照以下方式配置

### 3 配置


创建目录用来存放创建的虚拟环境

```bash
mkdir ~/.virtualenvs
```

查看 `virtualenvwrapper.sh` 所在目录

```bash
type virtualenvwrapper.sh
# 上面搜索不到就用下面指令
sudo find / -name virtualenvwrapper.sh
```

打开 `.bashrc` 文件, 如果是在图形系统下操作可以使用 `gedit` 打开, 这里使用 `vim`

```bash
vim ~/.bashrc
```

参照官方文档配置 `.bashrc` 文件，把以下代码放到 `~/.bashrc` 文件最后面

```bash
# 就是把 virtualenvwrapper.sh 所在的目录添加到 .bashrc 中
export WORKON_HOME=$HOME/.virtualenvs
VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export PATH=$PATH:/home/wz/.local/bin
source /home/wz/.local/bin/virtualenvwrapper.sh
```

如果使用的是root用户

```bash
export WORKON_HOME=$HOME/.virtualenvs
VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
# export PROJECT_HOME=$HOME/Devel
source /usr/local/bin/virtualenvwrapper.sh

# 注意: 使用root用户配置的虚拟环境, 别的用户是不能使用的, 需要到用户目录下的.bashrc中再把上面的代码粘贴进去, 然后刷新
```

保存退出后, 执行下面命令

```bash
source ~/.bashrc

# 如果出现, 表示配置成功
virtualenvwrapper.user_scripts creating /home/wz/.virtualenvs/premkproject
virtualenvwrapper.user_scripts creating /home/wz/.virtualenvs/postmkproject
virtualenvwrapper.user_scripts creating /home/wz/.virtualenvs/initialize
virtualenvwrapper.user_scripts creating /home/wz/.virtualenvs/premkvirtualenv
virtualenvwrapper.user_scripts creating /home/wz/.virtualenvs/postmkvirtualenv
virtualenvwrapper.user_scripts creating /home/wz/.virtualenvs/prermvirtualenv
virtualenvwrapper.user_scripts creating /home/wz/.virtualenvs/postrmvirtualenv
virtualenvwrapper.user_scripts creating /home/wz/.virtualenvs/predeactivate
virtualenvwrapper.user_scripts creating /home/wz/.virtualenvs/postdeactivate
virtualenvwrapper.user_scripts creating /home/wz/.virtualenvs/preactivate
virtualenvwrapper.user_scripts creating /home/wz/.virtualenvs/postactivate
virtualenvwrapper.user_scripts creating /home/wz/.virtualenvs/get_env_details
```

### 4 常用命令

列出所有虚拟环境

```bash
workon
```

创建新的虚拟环境, 创建后自动进入虚拟环境

```bash
mkvirtualenv 虚拟环境名
```

退出虚拟环境

```bash
deactivate
```

再次进入虚拟环境

```bash
workon 虚拟环境名
```

删除虚拟环境

```bash
rmvirtualenv 虚拟环境名
```

> 注意
>
> 当进入虚拟环境后, 不能删除所在的虚拟环境, 要先退出


