# 使用文档 - 使用gitea搭建git服务器


搭建属于自己的git服务器, 不用推送到GitHub或者Gitee

<!--more-->

### Windows版

#### 下载

官网: https://dl.gitea.io/

GitHub: https://github.com/go-gitea/gitea/releases

根据自己的系统下载, 这里选择的版本: gitea-1.15.6-windows-4.0-amd64.exe

下载好以后把文件名改为 `gitea.exe`

#### 注册为Windows服务

在你想要的地方创建一个 `Gitea` 目录, 用来存放 `gitea.exe`

要注册为Windows服务，首先以 `Administrator` 身份运行 `cmd`，然后执行以下命令

```bash
# 下面为命令格式
sc create gitea start= auto binPath= "\"你gitea.exe的路径\" web --config \"你Gitea目录的路径\custom\conf\app.ini\""

# 比如, 下面是我的路径, 我是把 gitea.exe 放在了 C:\All\IT\Gitea 目录下
sc create gitea start= auto binPath= "\"C:\All\IT\Gitea\gitea.exe\" web --config \"C:\All\IT\Gitea\custom\conf\app.ini\""
```

#### 运行

`Windows+R` 调出运行界面, 输入 `services.msc` 回车跳到服务窗口, 找到 `gitea`，右键启动

在浏览器打开 http://localhost:3000 就可以访问了, 如果你修改了端口，请访问对应的端口，3000是默认端口

> 提示
>
> Windows版本推荐使用SQLite

#### 删除

先删除Windows服务, 以 `Administrator` 身份运行 `cmd`, 执行以下命令, 然后删除 `gitea.exe `

```bash
sc delete gitea
```

### Ubuntu版

#### 服务启动版

参考文档: https://docs.gitea.io/en-us/install-from-binary/

系统版本: ubuntu 18.04

gitea版本: gitea-1.15.6-linux-amd64

MySQL版本: MySQL 8.0

> 提示
>
> 如果没有安装MySQL, 请跳过 **创建 MySQL用户** 这些步骤

##### 创建 MySQL 用户

登录数据库

```bash
mysql -uroot -p  # 输入密码
```

创建gitea用户

```mysql
SET old_passwords=0;  # MySQL8.0不用执行这一句
CREATE USER 'gitea' IDENTIFIED BY '你想设置的密码';
```

创建gitea数据库, 这里采用utf8mb4而不是utf8作为字符集, 是因为utf8mb4支持的Unicode字符更多

```mysql
CREATE DATABASE gitea CHARACTER SET 'utf8mb4' COLLATE 'utf8mb4_unicode_ci';
```

给gitea用户赋予数据库的权限, 刷新并退出

```mysql
GRANT ALL PRIVILEGES ON gitea.* TO 'gitea';
FLUSH PRIVILEGES;
exit
```

测试数据库连接

```bash
mysql -ugitea -p  # 输入密码
```

> 提示
>
> 这里的用户名, 密码, 数据库名等下会在创建站点时用到

##### 安装gitea

以下操作全在 `root` 用户下执行

```bash
cd  # 此时应该在当前用户目录下

su  # 输入密码后, 不要再进入其他目录了
```

**下载 gitea**

使用wget下载gitea二进制文件, 可以到官网或github下载

```bash
wget -O gitea https://dl.gitea.io/gitea/1.15.6/gitea-1.15.6-linux-amd64

# 下载完成后给文件赋予权限
chmod +x gitea

# 将 gitea 二进制文件移动到指定路径
mv gitea-1.15.6-linux-amd64 /usr/local/bin/gitea
```

**准备环境**

检查服务器上是否安装了 Git, 如果不是, 请先安装它

```bash
git --version
```

创建用户 `git` 用来运行 gitea

```bash
# 添加一个名为 git 的用户
adduser \
   --system \
   --shell /bin/bash \
   --gecos 'Git Version Control' \
   --group \
   --disabled-password \
   --home /home/git \
   git
```

创建所需的目录结构

```bash
mkdir -p /var/lib/gitea/{custom,data,log}
chown -R git:git /var/lib/gitea/
chmod -R 750 /var/lib/gitea/
mkdir /etc/gitea
chown root:git /etc/gitea
chmod 770 /etc/gitea
```

> 注意
>
> 为了`git`这个用户, 暂时的把目录`/etc/gitea`设置了写权限, 以便安装的时候程序可以写入配置文件, **安装完成后**, 建议使用以下方法将权限设置为只读, 下面两句指令暂时不执行
>
> ```bash
> chmod 750 /etc/gitea
> chmod 640 /etc/gitea/app.ini
> ```
>

**运行 gitea**

这里采用官方推荐的方法, 通过创建一个**服务文件**来自动启动 `gitea`, 在 `/etc/systemd/system` 目录下创建一个名为 `gitea.service` 的文件

```
vim /etc/systemd/system/gitea.service
```

将以下内容复制到 `gitea.service` 中，本文采用的是`MySQL`数据库, 需要把`MySQL`的注释打开，如果采用其他的数据库, 可以打开相关配置的注释, 并将`MySQL`的配置注释了

```
[Unit]
Description=Gitea (Git with a cup of tea)
After=syslog.target
After=network.target
###
# Don't forget to add the database service dependencies
###
#
Wants=mysql.service
After=mysql.service
#
#Wants=mariadb.service
#After=mariadb.service
#
#Wants=postgresql.service
#After=postgresql.service
#
#Wants=memcached.service
#After=memcached.service
#
#Wants=redis.service
#After=redis.service
#
###
# If using socket activation for main http/s
###
#
#After=gitea.main.socket
#Requires=gitea.main.socket
#
###
# (You can also provide gitea an http fallback and/or ssh socket too)
#
# An example of /etc/systemd/system/gitea.main.socket
###
##
## [Unit]
## Description=Gitea Web Socket
## PartOf=gitea.service
##
## [Socket]
## Service=gitea.service
## ListenStream=<some_port>
## NoDelay=true
##
## [Install]
## WantedBy=sockets.target
##
###

[Service]
# Modify these two values and uncomment them if you have
# repos with lots of files and get an HTTP error 500 because
# of that
###
#LimitMEMLOCK=infinity
#LimitNOFILE=65535
RestartSec=2s
Type=simple
User=git
Group=git
WorkingDirectory=/var/lib/gitea/
# If using Unix socket: tells systemd to create the /run/gitea folder, which will contain the gitea.sock file
# (manually creating /run/gitea doesn't work, because it would not persist across reboots)
#RuntimeDirectory=gitea
ExecStart=/usr/local/bin/gitea web --config /etc/gitea/app.ini
Restart=always
Environment=USER=git HOME=/home/git GITEA_WORK_DIR=/var/lib/gitea
# If you install Git to directory prefix other than default PATH (which happens
# for example if you install other versions of Git side-to-side with
# distribution version), uncomment below line and add that prefix to PATH
# Don't forget to place git-lfs binary on the PATH below if you want to enable
# Git LFS support
#Environment=PATH=/path/to/git/bin:/bin:/sbin:/usr/bin:/usr/sbin
# If you want to bind Gitea to a port below 1024, uncomment
# the two values below, or use socket activation to pass Gitea its ports as above
###
#CapabilityBoundingSet=CAP_NET_BIND_SERVICE
#AmbientCapabilities=CAP_NET_BIND_SERVICE
###

[Install]
WantedBy=multi-user.target

```

**启动gitea服务**

```bash
systemctl enable gitea
systemctl start gitea
```

**安装界面**

打开浏览器访问页面: http://你的服务器ip:3000/, 此时进入 gitea 的安装页面, 首先需要设置

开始是数据库设置, 填入上面创建的MySQL用户, 密码和数据库名, 如果没安装MySQL就选择SQLite3

{{< image src="https://pic.imgdb.cn/item/629a4b460947543129566e1f.png" caption="" width=100%  height="630" >}}

> 提示
>
> 如果只在本地使用, 数据库主机不用修改

然后是一般设置, 站点名称可以根据自己的需要修改, 如果只在本机使用, 其他选项不要改动, 如果在服务器中配置, 请参照下面**docker-compose版**图片中的配置

{{< image src="https://pic.imgdb.cn/item/629a4b460947543129566e22.png" caption="" width=100% height="600" >}}

然后是可选设置, 把Gravatar禁用了, 国内访问不了

{{< image src="https://pic.imgdb.cn/item/629a4b460947543129566e2b.png" caption="" width=100% height="600" >}}

点击管理员帐号设置展开选项, 可以设置管理员用户, 在这里不设置也可以, 默认会把创建的第一个用户作为管理员用户

{{< image src="https://pic.imgdb.cn/item/629a4b460947543129566e35.png" caption="" width=100% height="350" >}}

点击立即安装, 你就有了自己的git服务器, 剩下的操作与GitHub一样, 就不过多的介绍了

> 提示
>
> 网站已经创建成功, 别忘了关闭文件权限

**关闭权限**

需要关闭上面为了`git`这个用户暂时开启的写权限

```bash
chmod 750 /etc/gitea
chmod 640 /etc/gitea/app.ini
```

##### 修改配置

如果想修改配置, 可以通过修改配置文件进行修改, 比如想禁止用户注册

```bash
vim /etc/gitea/app.ini

# 找到服务配置, 更改为true
[service]
DISABLE_REGISTRATION              = true
```

更改后需要重载

```bash
systemctl daemon-reload
# 如果不行就使用
systemctl restart gitea
```

这时候在Gitea主页就没有注册的入口了

#### Docker-compose版

##### 安装docker-compose

使用二进制包安装很简单

```bash
# 直接到官网下载: https://github.com/docker/compose/releases/, 使用 xftp 上传到服务器
# 把文件移动到 /usr/local/bin/ 并修改文件名为 docker-compose
# 然后执行下面命令
sudo chmod +x /usr/local/bin/docker-compose
```

查看版本

```bash
docker-compose --version
```

卸载也很简单, 只需要删除文件

```bash
sudo rm /usr/local/bin/docker-compose
```

##### 安装Gitea

参照官方文档: https://docs.gitea.io/zh-cn/install-with-docker/#基本

**安装**

```bash
# 在想要的位置创建 Gitea 目录, 进入后创建 docker-compose.yml 文件
cd ~
mkdir Gitea
vim docker-compose.yml

# 然后把下面的内容粘贴进去, 保存退出

version: "3"

networks:
  gitea:
    external: false

services:
  server:
    image: gitea/gitea:1.15.7
    container_name: gitea
    environment:
      - USER_UID=1000
      - USER_GID=1000
    restart: always
    networks:
      - gitea
    volumes:
      - ./gitea:/data
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    ports:
      - "13000:3000"
      - "10022:22"
```

**启动**

```bash
# 以在后台启动 Gitea
docker-compose up -d
```

**其他命令**

```bash
# 显示 Gitea 是否正确启动
docker-compose ps
# 查看日志
docker-compose logs 
# 关闭设置, 这将停止并杀死容器, 这些卷将仍然存在
docker-compose down
```

**安装界面**

使用浏览器访问 http://server-ip:13000 , 注意某些地方需要安装下图中来配置

{{< image src="https://pic.imgdb.cn/item/629a4b460947543129566e44.png" caption="" width=100% height="450">}}

点击安装, 剩下的操作跟GitHub的操作一样

> 注意
>
> 1.如果点击安装时, 上图中**以用户名运行**报错, 按照上面**服务启动版-->安装gitea-->准备环境**中的步骤创建**git用户**
>
> 2.配置文件在: /home/wz/Gitea/gitea/gitea/conf/app.ini

**测试功能**

1. 登录gitea服务器, 创建一个仓库
2. 点击右上角头像-->设置-->SSH/GPG秘钥-->增加秘钥
3. 然后把电脑中的公钥复制进去
4. 在电脑中新建目录, 把仓库clone下来
4. 新建文件, 执行 git add . --> git commit -m "" --> git push, 然后查看远程仓库


