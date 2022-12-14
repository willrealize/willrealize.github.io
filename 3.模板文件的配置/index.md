# Hugo文档 - 3. 模板文件的配置


模板文件的配置和头部参数的解释

<!--more-->

### 创建文件的命令

首先从创建文件的命令说起

```bash
# 使用命令生成文件
hugo new posts/filename.md
```

执行上面命令, 会使用 archetypes/default.md 模板进行创建文件

### 模板

#### 初始模板

{{< image src="https://pic.imgdb.cn/item/629a49e70947543129551843.png" caption="" width="900" height="120" >}}

`title`: 文章标题, 显示在主页的

`date`: 文章创建日期

`draft`: 是否为草稿

官方做法: 当使用命令创建的时候, 文件为草稿, 就是 `draft: true`, 执行 `hugo server` 开启服务, 打开本地网站后, 是看不到新写的文章的, 需要执行 `hugo server -D` 或 `hugo server --buildDrafts` 才能看到, 写好文章后再修改为 false, 然后生成静态网页发布

> 注意
>
> 如果不修改为 `false` , 把执行 `hugo` 命令生成的 `public` 目录部署到 `GitHub`上, 打开 https://willrealize.github.io/ 是看不到这篇文章的

当然, 模板可以修改!

#### 我的模板

{{< image src="https://pic.imgdb.cn/item/629a49e7094754312955185b.png" caption="" width="900" height="500" >}}

参照官方文档: https://hugodoit.pages.dev/zh-cn/theme-documentation-content/

`<!--more-->`: 摘要分隔符, 摘要分隔符之前的内容将用作该文章的摘要

`authors`: 作者, 可以有多个, 如果要配置多个作者, 在 `mysite/data` 下创建 `authors` 目录, 目录下面创建每个作者的配置文件

`lastmod`: 最后修改时间, 当写完或再次修改文章时, 修改一下这个参数的值

`draft`: 我是设置为 `false` , 这样就可以直接使用 `hugo server` 开启服务, 如果不想发布, 则改为 `true`

`categories`: 分类功能, 想把文章放在哪个分类, 就把分类名写在后边

`tags`: 标签功能, 想把文章想打上什么标签, 就把标签名写在后边

`toc`: 是用来配置文章页面目录的, 还有子参数:

- `auto`: 是用来控制目录是否自动折叠, 默认设置为 `false` 不自动折叠, 如果目录太长了, 可以配置为 `true`
- `enable`: 默认是打开的, 使用目录功能就设置为 `true`, 不想使用就设置为 `false`, 暂时不用
- `keepStatic`: 不保持使用文章前面的静态目录就设置为 `false`, 这样目录就会显示在右边, 暂时不用

`comment`: 评论功能, 默认是打开的, 有些页面不想设置评论功能可以设置为 `false` 关闭

`lightgallery`: 画廊功能, 必须与图片语法配合使用: \{\{< image src="图片路径" caption="图片标题" width="宽" height="高" >}}

`featuredImage`: 显示在文章最上方和主页的特色图片


当然还有很多别的参数, 可以查看上面的官方文档(在 **3 前置参数** 中), 计划以后会添加一些别的参数, 比如: 

```yaml
# 这三个是一起使用的, 用来配置文章所属系列的
series: []
series_weight: 1
seriesNavigation: true
```




