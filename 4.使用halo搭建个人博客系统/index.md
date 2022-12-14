# 使用文档 - 使用Halo搭建个人博客网站


体验一下Halo博客系统, 环境: centos 8.5.2111, docker 20.10.11, halo 1.4.13

<!--more-->

### 安装Docker

> 注意
>
> 1 安装 `Docker` 请参照官网: https://docs.docker.com/engine/install/centos/
>
> 2 这里用的是 `root` 用户, 如果是非 `root` 用户, 下面的命令前面要加 `sudo`

可以先升级一下 `yum`

```bash
yum update
```

#### 安装

```bash
yum install -y yum-utils

yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

yum install docker-ce docker-ce-cli containerd.io
```

如果需要卸载

```bash
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

#### 启动

```bash
systemctl start docker

# 其他命令
systemctl restart docker  # 重启
systemctl stop docker  # 停止
systemctl status docker  # 查看状态
```

#### 测试

```bash
docker run hello-world

# 看到以下信息代表安装成功
[root@centos ~]# docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete 
Digest: sha256:cc15c5b292d8525effc0f89cb299f1804f3a725c8d05e158653a563f15e4f685
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

### 安装 halo

官方文档: https://docs.halo.run/getting-started/install/docker

Halo 在 Docker Hub 上发布的镜像为 `halohub/halo`: https://hub.docker.com/r/halohub/halo

#### 创建工作目录

查看什么是[工作目录](https://docs.halo.run/getting-started/prepare#工作目录)

```bash
mkdir ~/.halo
```

#### 拉取 Halo 镜像

```bash
docker pull halohub/halo:1.4.13
```

> 查看最新版本镜像: https://hub.docker.com/r/halohub/halo, 官方推荐使用具体版本号的镜像，但也提供了 `latest` 标签的镜像，它始终是最新的。

#### 创建容器

```bash
docker run -it -d --name halo -p 8090:8090 -v ~/.halo:/root/.halo --restart=unless-stopped halohub/halo:1.4.13
```

`-it`: 开启输入功能并连接伪终端

`-d`: 后台运行容器

`--name`: 为容器指定一个名称

`-p`: 端口映射，格式为 `主机(宿主)端口:容器端口` ，可在 `application.yaml` 配置

`-v`: 工作目录映射。形式为：`-v` `宿主机路径`:`/root/.halo`，后者不能修改

`--restart`: 建议设置为 `unless-stopped`，在 Docker 启动的时候自动启动 Halo 容器

#### 打开网站

在终端输入指令查看 `ip地址`

```bash
ifconfig
```

打开 `http://ip:8090` 即可看到安装引导界面

### 博客后台

#### 设置

打开网站后进入设置页面, 配置完成后点击安装, 如下图

{{< image src="https://pic.imgdb.cn/item/629a4b50094754312956782a.png" caption="" width="1550" height="700" >}}

> 提示
>
> 后台地址: http://192.168.31.220:8090/admin/index.html#/dashboard

进入登录页面, 输入用户密码点击登录

{{< image src="https://pic.imgdb.cn/item/629a4b50094754312956783d.png" caption="" width="1550" height="400" >}}

下面就是 Halo Dashboard , 博客的后台

{{< image src="https://pic.imgdb.cn/item/629a4b5a0947543129568287.png" caption="" width="1550" height="500" >}}

点击上图右上角三个按钮中的第一个, 就可以查看博客首页

#### 主题

查看主题在下图中的 外观 - 主题

{{< image src="https://pic.imgdb.cn/item/629a4b5a094754312956828a.png" caption="" width="1550" height="450" >}}

点击上图中的安装按钮出现下面弹出框, 这里选择使用远程下载来安装主题

{{< image src="https://pic.imgdb.cn/item/629a4b5a0947543129568295.png" caption="" width="1550" height="400" >}}

安装好后如下图, 点击启用, 再点击设置就可以设置主题了

{{< image src="https://pic.imgdb.cn/item/629a4b5a094754312956829d.png" caption="" width="1550" height="400" >}}

去探索更多吧, 可以从写一篇文章开始!

### 部署

这里部署使用的是云服务器, 系统是Ubuntu18.04

#### 安装 Nginx

```bash
su root
apt install nginx
```

安装后测试一下: 如果是云服务器, 要先到控制台打开安全组中的80端口, 然后在浏览器中输入服务器网址, 出现Nginx的欢迎页面代表已经安装成功

#### 安装目录

使用apt安装的Nginx的目录使用编译安装的目录不同, 下面为apt安装Nginx的目录

- /usr/sbin/nginx： 主程序
- /usr/share/nginx： 存放静态文件
- /etc/nginx： 存放配置文件
- /var/log/nginx： 存放日志
- /var/www/: 存放项目或站点
- /etc/nginx/conf.d: 配置也可以放到这个目录

#### 配置证书

##### 申请证书

到购买服务器的厂商申请, 在搜索框输入 **SSL证书** 进行搜索, 根据指导操作

##### 上传至服务器

上传证书后, 把证书压缩文件解压, 里面有多种服务器的证书, 找到其中存放 Nginx证书 的目录, 把证书移动到 Nginx 的安装目录下

```bash
# 新建一个存放证书的目录, 可以存放多个证书
mkdir /etc/nginx/certs
# 新建存放主域名证书的目录
mkdir /etc/nginx/certs/main
# 进入 Nginx证书目录
cd Nginx证书目录
# 移动到Nginx安装目录下
mv ./* /etc/nginx/certs/main
```

##### 新建配置

Ubuntu 中, 使用 apt 安装的 Nginx, 主配置文件是 /etc/nginx/nginx.conf, 这个文件一般用来配置公用的功能和不常修改的功能, 一般不在这个地方进行具体的配置, 为了不让越来越多的配置导致项目混乱, 会使用目录管理配置, 配置目录有两个:

- /etc/nginx/sites-available: 默认有一个配置模板文件default, 文件中也描述了怎么使用自己创建的配置文件, 如果需要添加配置, 先在这个目录下新建一个名为 xxx 的配置文件, 然后到 /etc/nginx/sites-enabled 目录下使用 ln -s /etc/nginx/sites-available/xxx 添加软链接, 如果不想使用配置, 直接删除软链接即可, 文件名后缀可以使用 .conf, 也可以不用后缀
- /etc/nginx/conf.d: 使用方法是在此目录下直接创建一个配置文件, 只不过文件名后缀必须为 .conf

```bash
cd /etc/nginx/sites-available
# 一般不使用 default 这个文件, 先备份了, 然后删除软连接
mv defualt ./defualt.back
rm /etc/nginx/sites-enabled/defualt
```

下面以配置主域名为例, 使用的配置目录为 /etc/nginx/sites-available

```bash
# 先添加一个名为 xxx.com 的文件
vim xxx.com

# 添加下面内容
upstream halo {  # 负载均衡, 官方文档是这样写的, 个人网站很少用到
        server 127.0.0.1:8090;
}
server {  # 用来把http请求转为https请求, 注: 每个子域名都要配置
        listen 80;
        server_name xxx.com www.xxx.com;
        client_max_body_size 1024m;
        rewrite ^(.*) https://$server_name$1 permanent;
}
server {
        listen 443 ssl;
        ssl on;
        server_name xxx.com www.xxx.com;
        
        ssl_certificate /etc/nginx/certs/main/scsxxx_www.xxx.com_server.crt;
        ssl_certificate_key /etc/nginx/certs/main/scsxxx_www.xxx.com_server.key;
        
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
        ssl_prefer_server_ciphers on;
        location / {
                proxy_pass http://halo;  # 反向代理
                proxy_set_header HOST $host;
                proxy_set_header X-Forwarded-Proto $scheme;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
}

# 保存退出后创建软链接
cd /etc/nginx/sites-enabled
ln -s /etc/nginx/sites-available/xxx.com

# 可以先测试一下配置文件, 如果报错就根据报错修改
nginx -t

# 重新加载Nginx
systemctl reload nginx
# 或
nginx -s reload

# 然后就可以到浏览器打开 xxx.com 了
```


