# Hugo文档 - 2. 部署个人网站到GitHub


新申请的github账号, 使用公钥与windows建立连接, 把public目录推送到远程仓库

<!--more-->

### 1 创建仓库

#### 先在Git hub上新建仓库

**进入GitHub, 点击右上角图标, 新建仓库**

{{< image src="https://pic.imgdb.cn/item/629a49d0094754312954f8a5.png" caption="" width="900" height="188" >}}

**填写仓库名, 必须以 用户名.github.io 这种格式, 且必须公开**

{{< image src="https://pic.imgdb.cn/item/629a49d0094754312954f8bf.png" caption="" width="1500" height="350" >}}

**点击确认后创建成功**

{{< image src="https://pic.imgdb.cn/item/629a49d7094754312955023b.png" caption="" width="1500" height="288" >}}

#### 进入public目录并初始化

```bash
git init
```

**把public提交到本地仓库**

```bash
git add .
git commit -m "new site"
```

### 2 生成秘钥

**执行下面指令后连续确认, 直到生成图案**

```bash
ssh-keygen -t rsa -C "13215686668@qq.com"
```

{{< image src="https://pic.imgdb.cn/item/629a49e00947543129551077.png" caption="" width="1000" height="350" >}}

**在C盘用户目录下找到.ssh目录, 可以看到里面生成了一个秘钥一个公钥, 秘钥不用管, 把公钥里面的内容拷贝下来**

{{< image src="https://pic.imgdb.cn/item/629a49d70947543129550254.png" caption="" width="1000" height="130" >}}

### 3 把公钥放到git hub上

**进入GitHub, 点击右上角图标, 点击settings**

{{< image src="https://pic.imgdb.cn/item/629a49d70947543129550241.png" caption="" width="1000" height="450" >}}

**再点击左边选项: SSH and GPG keys**

{{< image src="https://pic.imgdb.cn/item/629a49d7094754312955025c.png" caption="" width="1000" height="180" >}}

**点击 New SSH key**

{{< image src="https://pic.imgdb.cn/item/629a49d70947543129550268.png" caption="" width="1000" height="200" >}}

**出现新的页面, Title随便写, Key就是用户目录下的公钥, 打开公钥文件, 把内容全部复制过来**

{{< image src="https://pic.imgdb.cn/item/629a49e00947543129551066.png" caption="" width="1000" height="320" >}}

**点击 Add SSH key 创建完成**

{{< image src="https://pic.imgdb.cn/item/629a49e0094754312955106c.png" caption="" width="1000" height="230" >}}

### 4 建立连接

**打开git bash进入public目录下, 用 ssh -T git@github.com , 测试是否能够连接成功, 如果使用码云: ssh -T git@gitee.com**

{{< image src="https://pic.imgdb.cn/item/629a49e00947543129551080.png" caption="" width="1000" height="135" >}}

**本地仓库关联远程仓库: git remote add origin 远程仓库的地址, 指令在建立远程仓库成功时可以看到**

{{< image src="https://pic.imgdb.cn/item/629a49d7094754312955023b.png" caption="" width="1000" height="300" >}}

**这里选择SSH方式**

```bash
git remote add origin git@github.com:willrealize/willrealize.github.io.git
```

**关联了远程仓库之后, 我们可以把本地的仓库的内容推上去, 第一次推送要加 -u, 以后就可以直接使用 git push , 出现下面的内容, 则本地仓库的内容已经推到git hub上了**

```bash
git push -u origin master
```

{{< image src="https://pic.imgdb.cn/item/629a49e0094754312955108b.png" caption="" width="1000" height="200" >}}

**等待一分钟, 打开 https://willrealize.github.io/ , 就可以看到博客了**

{{< image src="https://pic.imgdb.cn/item/629a49d0094754312954f80b.png" caption="" width="1000" height="500" >}}

### 5 碰到的问题

如果上传到GitHub, 隔一段时间打开还没有刷新网站, 要查看代码提交页面, 如下图, 如果不是对号(√)是错号(×), 说明提交的某处有问题, GitHub不会帮你更新到最新提交的页面, 要点击错号查看错误, 一般有错误报告, 跟着报告定位问题, 解决问题后重新生成 public 目录提交

{{< image src="https://pic.imgdb.cn/item/629a49c7094754312954ec75.png" caption="" width="1000" height="300" >}}

