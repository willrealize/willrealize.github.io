# Hugo文档 - 7. 使用云服务器搭建个人网站


使用github pages访问网页的速度并不理想, 决定使用云服务器来搭建个人网站

<!--more-->

### 准备工作

系统: 阿里云服务器 ubuntu 18.04

工具: xshell

思路: 把本地的 `public` 目录推送到云服务器中创建的仓库`hugo.git`中, `hugo.git`只存提交信息, 静态资源会存在 `hugo`目录中, 使用 `Nginx` 作反向代理, 当用户访问服务器时, 服务器把 HTTP 请求转发给 Nginx, Nginx 把`hugo`目录下的静态资源返回给用户

{{< image src="https://gitee.com/willrealize/images/raw/master/hugo_images/hugo/示意图.png" width="100%" caption="示意图" >}}

### 连接服务器

使用 xshell 或者 cmd 连接到云服务器, 如果使用 cmd

```bash
# 如果未创建用户, 默认为 root 用户
ssh root@服务器ip

# 如果已经创建了用户
ssh 用户名@服务器ip
```

### 检查是否安装 git

```bash
git version

# 如果没安装
sudo apt install git
```

### 创建 git 用户

```bash
sudo adduser git
```

关于用户的其他指令, 这里暂时不用

```bash
# 设置用户密码, 如果是普通用户可以用来修改密码
passwd 用户名
# 删除用户
sudo userdel -r 用户名
```

### 创建名为 git 的"裸仓库"

创建仓库 hugo.git, 创建存放源码的目录 hugo

```bash
# 使用git用户
su git
# 进入git用户目录下
cd /home/git
# 创建一个git目录
mkdir git
cd git
# 此时所在的目录是 /home/git/git
git init --bare hugo.git
# 分配目录的拥有者
sudo chown -R git:git hugo.git
mkdir hugo
```

> 注意
>
> 执行 `git init –bare hugo.git` 会生成一个名为`hugo.git`的"裸仓库", 所谓的"裸仓库"是没有工作区的, 只会记录 git 提交的历史信息, `git log`一下是可以看到各个版本信息的, 但是没办法进行版本回退或者切换分支的操作, 但是有一个好处是可以通过添加 hooks 钩子, 然后在同级目录下新建一个目录 hugo 用来存放项目源码, 也就是说将 git 仓库与项目源码分离

### 配置钩子

`post-receive` 钩子在整个过程完结以后运行, 可以用来更新其他系统服务或者通知用户

```bash
vim /home/git/git/hugo.git/hooks/post-receive
```

把下面代码复制到里面

```bash
git --work-tree=/home/git/git/hugo --git-dir=/home/git/git/hugo.git checkout -f
```

配置权限

```bash
chmod +x /home/git/git/hugo.git/hooks/post-receive
```

### 配置Nginx

安装Nginx

```bash
sudo apt update 
sudo apt install nginx
```

修改Nginx默认的配置文件

```bash
vim /etc/nginx/sites-available/default
# 把 root /var/www/html 改成 /home/git/git/hugo
```

重启服务

```bash
service nginx reload
service nginx restart
# 使用普通账号启动或重载系统服务, 会提示需要输入密码, 根据提示输入密码即可
```

关于Nginx的其他指令, 这里暂时不用

```bash
# 删除除了配置文件以外的所有文件
sudo apt remove nginx nginx-common

# 删除所有与nginx有关的东西, 包括配置文件。
sudo apt purge nginx nginx-common 

# 在上面命令结束后执行, 主要是删除与Nginx有关的且不再被使用的依赖包
sudo apt autoremove 

# 删除两个主要的包。
sudo apt remove nginx-full nginx-common 

# 重启nginx, 重启失败, 说明已成功卸载nginx
sudo service nginx restart
```

### 建立连接

道理跟连接 GitHub 一样

先配置SSH公钥, 在本地电脑执行下面命令, 会在用户目录下生成一个 .ssh 的目录, 打开 id_rsa.pub 并复制内容

```bash
ssh-keygen -t RSA -C "邮箱地址"
```

在服务器中创建 .ssh 目录, 再创建文件 authorized_keys  用来存放公钥

```bash
# 此时所在的目录是 /home/git/git
mkdir .ssh
touch .ssh/authorized_keys
chmod 600 .ssh/authorized_keys

# 打开文件, 把 id_rsa.pub 里面的公钥复制到里面
vim .ssh/authorized_keys
```

### 推送

如何配置站点和生成public目录就不多说了

在mysite下执行 `hugo`后,  把 `public`目录推送到服务器中的 `hugo.git` 仓库里面

```bash
cd public
git init
git add .
git commit -m '使用云服务器创建个人网站'  # 如果没有设置用户和邮箱, 根据提示设置
git remote add origin git@服务器ip:/home/git/git/hugo.git
git push -u origin master  # 提示输入密码, 输入 git 用户的密码
```

在浏览器中输入你的域名( 如果没有购买域名, 就输入服务器IP地址)就能查看到网站了

### 附

菜鸟篇

使用 xftp 把 public 目录传送到服务器上, 修改/etc/nginx/sites-available/default, 把 root /var/www/html 改成     public 目录所在路径, 重载服务器 service nginx reload, service nginx restart, 然后在浏览器中输入服务器ip地址访问

