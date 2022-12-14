# Hugo文档 - 6. 使用Gitalk给网站增加评论功能


使用Gitalk作为博客网站的评论系统

<!--more-->

### 创建新应用 

打开 `https://github.com/settings/applications/new` , 根据下图填写

{{< image src="https://pic.imgdb.cn/item/629a4b350947543129565da3.png" caption="" width="1550" height="550" >}}

填好之后点击`Register application`进入下图中的页面, 我们需要下图中两个参数, 如果没有 `Client  secret` , 按照下图新建一个

{{< image src="https://pic.imgdb.cn/item/629a4b350947543129565da7.png" caption="" width="1550" height="550" >}}

创建好后如下图

{{< image src="https://pic.imgdb.cn/item/629a4b350947543129565db1.png" caption="" width="1550" height="330" >}}

> 注意
>
> 立即把 `Client secret` 拷贝下来, 不然就看不到了

如果你需要, 也可以给应用添加一个图标

{{< image src="https://pic.imgdb.cn/item/629a4b350947543129565db9.png" caption="" width="1550" height="400" >}}

如果想修改应用, 修改以后点击 `Update application`

{{< image src="https://pic.imgdb.cn/item/629a4b350947543129565dc5.png" caption="" width="1550" height="530" >}}

### 配置评论功能

打开 `config.toml` , 找到下图中**评论系统设置**, `enable` 设置为 `true`

{{< image src="https://pic.imgdb.cn/item/629a4b3f09475431295666f7.png" caption="" width="1550" height="60" >}}

再找到下图中 **Gitalk 评论系统设置**, `enable` 设置为 `true`

{{< image src="https://pic.imgdb.cn/item/629a4b3f0947543129566700.png" caption="" width="1550" height="147" >}}

**参数解释:**

`owner`: 是你 GitHub 的`用户名`

`repo`: 是存放评论的`仓库名`, 需要自己新建一个, 名字随便取

`clientId`: 上面所创建应用的 `Client ID`

`clientSecret`: 上面所创建应用的 `Client secret`

Gitalk官方文档: `https://github.com/gitalk/gitalk`

> 注意
>
> `Client secrets` 下面可以有很多 `Client secret` , 只有新创建的时候能看到 `Client secret` 的值, 使用新建的

### 使用

根据上面所有步骤配置好以后, 启动本地服务

```bash
# hugo server 启动的是 development 环境, 评论功能不会在 development 环境下启用, 所以使用下面指令
hugo server -e production
```

打开本地网站, 点击某篇文章, 翻到评论区可以看到下图, 代表已经设置好了

{{< image src="https://pic.imgdb.cn/item/629a4b3f0947543129566707.png" caption="" width="1550" height="280" >}}

> 注意
>
> 这时点击 `使用 GitHub 登录` 按钮会跳转到 `https://willrealize.github.io/` , 而此时远程的网站中还没有设置评论功能, 需要先把本地新增的功能推送到 GitHub

生成静态文件并推送到 `GitHub`

```bash
# 在站点目录下
hugo

# 进入public目录, 推送到GitHub
cd public
git add .
git commit -m "增加评论功能"
git push
```

推送完成后, 等待一分钟, 打开 `https://willrealize.github.io/`, 随便点击一篇文章, 翻到评论区, 就会出现上一张图片的情况, 这时点击 `使用 GitHub 登录` 按钮就会跳到下图中的授权页面

{{< image src="https://pic.imgdb.cn/item/629a4b3f0947543129566718.png" caption="" width="1550" height="550" >}}

点击 `Authorize willrealize` 按钮进行授权, 授权后就可以使用评论功能了

<img src="https://pic.imgdb.cn/item/629a4b3f0947543129566723.png" style="zoom: 67%;" />

