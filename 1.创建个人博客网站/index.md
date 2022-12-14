# Hugo文档 - 1. 创建个人博客网站




使用Win11+Hugo+GitHub, 搭建个人博客网站!



<!--more-->



### 1 安装Go

- 打开 https://golang.google.cn/,  下载golang, 下载最新版本即可,  根据提示安装, 我的版本: 1.17.0



### 2 安装Hugo

- 打开 https://github.com/gohugoio/hugo/releases , 下载hugo, 我的版本: hugo_0.89.2_Windows-64bit.zip

- 下载后, 解压, 把 `hugo.exe` 放到想放的目录

- 把目录添加到环境变量

- 测试: 打开 `cmd` , 输入 `hugo version` , 返回版本号



### 3 创建本地站点

#### 创建站点

```bash
# 在你想要的地方创建一个目录, 打开cmd, 进入目录, 执行命令
hugo new site mysite

# 创建站点成功后的目录结构
mysite
	archetypes
	content
	data
	layouts
	resources
	static
	themes
	config.toml
```

#### 下载主题

```bash
# 下载主题, 这里使用的是 DoIt 主题:
cd mysite/themes
git clone https://github.com/HEIGE-PCloud/DoIt.git
```

#### 配置config.toml

- 打开config.toml并删除所有内容

- 打开网页 https://hugodoit.pages.dev/zh-cn/theme-documentation-basics/

- 把网页中, 步骤2.3和3.1中的配置复制到config.toml中, 下面有我的配置

#### 修改模板文件

```bash
# 打开archetypes/default.md, 修改文件头部配置参数
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: false
categories: []
```

#### 创建文章

```bash
# 进入创建的站点 mysite 目录下, 执行下面指令, 会在 content 目录下生成 posts 目录和目录里的 .md 格式文件
hugo new posts/first_post.md

# posts 就是以后写博客的目录, 在生成的 .md 文件中写博客
```

#### 启动项目

```bash
# 执行下面指令后, 在浏览器中打开 http://localhost:1313/ , 就可以看到创建的站点和新建的博客
hugo server
```

#### hugo指令

```bash
# 在站点目录 mysite 下执行下面指令, 生成 public 目录, 如果想部署到 GitHub 上, 只需把 public 目录推送到 GitHub
hugo

# 还可以指定主题和所要部署的网站地址
hugo --theme=DoIt --baseUrl="https://willrealize.github.io/"

# 如何部署, 会在下一章讲
```



### 4 本地调试碰到的问题

- 创建博客文件名不能有空格

- 新建博客要写内容, 不能为空, 否则启动不了服务

- 执行 `hugo server` ,   打开网站, 不显示博客, 需要把文件头部 `draft` 的值修改为 `false` 

- 修改博客文件名或增加文件后, 然后查看网页里面博客所在的分类, 文件名没有及时刷新, 重启一下服务就行了

- 如果想使用分类, 在 `default.md` 中头部添加一行: `categories: []` , 如果想使用标签, 也是一样: `tags: []`

- `hugo server` 的默认运行环境是 `development`, 而部署到 GitHub 的网站默认运行环境是 `production`.由于本地 `development` 环境的限制, 评论系统 , CDN 和 fingerprint 不会在 `development` 环境下启用. 可以使用 `hugo server -e production` 命令来开启这些功能

- 不要随意改动博客文件名: 执行 `hugo` 命令的时候会在 `public`目录下重新生成博客目录, 而不是覆盖之前的, 这样会造成空间浪费, 修改文件名的时候, 可以清空 `public` 目录重新生成

- 在部署好了网站后发现有两个问题: 

    1 当打开文章详情页的时候浏览器最底下有一行可以左右滑动的滚动条, 当鼠标下拉的时候就会消失. 

    2 右下角没有回到顶部的按钮. 

    问题已解决: 把 `themes\DoIt\exampleSite\resources\_gen` 文件夹拷贝到 `mysite\resources` 下面, 如果已经存在, 就覆盖掉

    


### 附: 我的配置

```toml
baseURL = "http://example.org/"
# [en, zh-cn, fr, ...] 设置默认的语言
# defaultContentLanguage = "en"
defaultContentLanguage = "zh-cn"
# 网站语言, 仅在这里 CN 大写
languageCode = "zh-CN"
# 是否包括中日韩文字
hasCJKLanguage = true
# 网站标题
title = "个人博客"

# 更改使用 Hugo 构建网站时使用的默认主题
theme = "DoIt"



[menu]
  [[menu.main]]
    identifier = "posts"
    # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
    pre = ""
    # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
    post = ""
    name = "文章"
    url = "/posts/"
    # 当你将鼠标悬停在此菜单链接上时, 将显示的标题
    title = ""
    weight = 1
  
  [[menu.main]]
    identifier = "categories"
    pre = ""
    post = ""
    name = "分类"
    url = "/categories/"
    title = ""
    weight = 2

#  [[menu.main]]
#    identifier = "tags"
#    pre = ""
#    post = ""
#    name = "标签"
#    url = "/tags/"
#    title = ""
#    weight = 3

  [[menu.main]]
    identifier = "about"
    pre = ""
    post = ""
    name = "关于"
    url = "/about/"
    title = ""
    weight = 4

  # github配置
  [[menu.main]]
    identifier = "github"
    pre = "<i class='fab fa-github fa-fw'></i>"
    post = ""
    name = ""
    url = "https://baidu.com"
    title = "我的GitHub"
    weight = 5




[params]
  # DoIt 主题版本
  version = "0.2.X"

  # 网站名称
  title = "个人博客"

  # 网站描述
  description = "这是我的全新 Hugo 网站"

  # 网站关键词
  keywords = ["Theme", "Hugo"]

  #  网站默认主题样式 ("light", "dark", "black", "auto")
  defaultTheme = "auto"

  # 公共 git 仓库路径, 仅在 enableGitInfo 设为 true 时有效
  gitRepo = ""

  #  哪种哈希函数用来 SRI, 为空时表示不使用 SRI
  # ("sha256", "sha384", "sha512", "md5")
  fingerprint = ""

  #  日期格式
  dateFormat = "2006-01-02"

  # 网站图片, 用于 Open Graph 和 Twitter Cards
  images = ["/logo.png"]

  #  开启 PWA 支持
  enablePWA = false

  #  版权信息
  # license = '<a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>'

  #  应用图标配置
  [params.app]
    # 当添加到 iOS 主屏幕或者 Android 启动器时的标题, 覆盖默认标题
    title = "DoIt"
    # 是否隐藏网站图标资源链接
    noFavicon = false
    # 更现代的 SVG 网站图标, 可替代旧的 .png 和 .ico 文件
    svgFavicon = "https://github.com/favicon.ico"
    # Safari 图标颜色
    iconColor = "#5bbad5"
    # Windows v8-10磁贴颜色
    tileColor = "#da532c"

  #  搜索配置
  [params.search]
    enable = true
    # 搜索引擎的类型 ("lunr", "algolia", "fuse")
    type = "fuse"
    # 文章内容最长索引长度
    contentLength = 4000
    # 搜索框的占位提示语
    placeholder = "输入文章标题或文章内一级标签"
    #  最大结果数目
    maxResultLength = 2
    #  结果内容片段长度
    snippetLength = 50
    #  搜索结果中高亮部分的 HTML 标签
    highlightTag = "em"
    #  是否在搜索索引中使用基于 baseURL 的绝对路径
    absoluteURL = false
    [params.search.algolia]
      index = ""
      appID = ""
      searchKey = ""
    [params.search.fuse]
      #  https://fusejs.io/api/options.html
      isCaseSensitive = false
      minMatchCharLength = 2
      findAllMatches = false
      location = 0
      threshold = 0.3
      distance = 100
      ignoreLocation = false
      useExtendedSearch = false
      ignoreFieldNorm = false

  # 页面头部导航栏配置
  [params.header]
    # 桌面端导航栏模式 ("fixed", "normal", "auto")
    desktopMode = "fixed"
    # 移动端导航栏模式 ("fixed", "normal", "auto")
    mobileMode = "auto"
    #  主题切换模式
    # 主题切换模式 ("switch", "select")
    themeChangeMode = "switch"
    #  页面头部导航栏标题配置
    [params.header.title]
      # LOGO 的 URL
      logo = ""
      # 标题名称
      name = "个人博客"
      # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
      pre = ""
      # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
      post = ""
      #  是否为标题显示打字机动画
      typeit = false

  # 页面底部信息配置
  [params.footer]
    enable = true
    #  自定义内容 (支持 HTML 格式)
    custom = 'Powered by <a href="https://gohugo.io/" target="_blank">Hugo</a>. Theme based on <a href="https://github.com/HEIGE-PCloud/DoIt" target="_blank">DoIt</a>!'
    #  是否显示 Hugo 和主题信息
    hugo = false
    #  是否显示版权信息
    copyright = true
    #  是否显示作者
    author = true
    # 网站创立年份
    since = 2019
    # ICP 备案信息, 仅在中国使用 (支持 HTML 格式)
    icp = ""
    # 许可协议信息 (支持 HTML 格式)
    # license = '<a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>'

  #  Section (所有文章) 页面配置
  [params.section]
    # section 页面每页显示文章数量
    paginate = 20
    # 日期格式 (月和日)
    dateFormat = "01-02"
    # RSS 文章数目
    rss = 10
    #  最近更新文章设置
    [params.section.recentlyUpdated]
      enable = false
      rss = false
      days = 30
      maxCount = 10

  #  List (目录或标签) 页面配置
  [params.list]
    # list 页面每页显示文章数量
    paginate = 20
    # 日期格式 (月和日)
    dateFormat = "01-02"
    # RSS 文章数目
    rss = 10

  # 主页配置
  [params.home]
    #  RSS 文章数目
    rss = 10

    # 主页个人信息
    [params.home.profile]
      enable = false

      # Gravatar 邮箱, 用于优先在主页显示的头像
      gravatarEmail = ""

      # 主页显示头像的 URL
      # 将你的头像文件放置于 static 或者 assets 目录下 注: 在 mysite/themes/DoIt/assets/images 下存放
      # 文件路径是相对于 static 或者 assets 目录的
      # avatarURL = "/images/avatar.webp"
      avatarURL = "/images/avatar.png"

      #  主页显示的网站标题 (支持 HTML 格式), 这个在头像下面的, 下面还有一个副标题
      title = ""

      # 主页显示的网站副标题
      subtitle = "悟已往之不谏, 知来者之可追!"

      # 是否为副标题显示打字机动画
      typeit = true

      # 是否显示社交账号
      social = true

      #  免责声明 (支持 HTML 格式), 这个在社交账号图标下面
      disclaimer = ""

    # 主页文章列表
    [params.home.posts]
      enable = true
      # 主页每页显示文章数量
      paginate = 8
      #  被 params.page 中的 hiddenFromHomePage 替代
      # 当你没有在文章前置参数中设置 "hiddenFromHomePage" 时的默认行为
      defaultHiddenFromHomePage = false

  # 作者的社交信息设置
  [params.social]
    GitHub = "xxxx"
    Linkedin = ""
    Twitter = "xxxx"
    Instagram = "xxxx"
    Facebook = "xxxx"
    Telegram = "xxxx"
    Medium = ""
    Gitlab = ""
    Youtubelegacy = ""
    Youtubecustom = ""
    Youtubechannel = ""
    Tumblr = ""
    Quora = ""
    Keybase = ""
    Pinterest = ""
    Reddit = ""
    Codepen = ""
    FreeCodeCamp = ""
    Bitbucket = ""
    Stackoverflow = ""
    Weibo = ""
    Odnoklassniki = ""
    VK = ""
    Flickr = ""
    Xing = ""
    Snapchat = ""
    Soundcloud = ""
    Spotify = ""
    Bandcamp = ""
    Paypal = ""
    Fivehundredpx = ""
    Mix = ""
    Goodreads = ""
    Lastfm = ""
    Foursquare = ""
    Hackernews = ""
    Kickstarter = ""
    Patreon = ""
    Steam = ""
    Twitch = ""
    Strava = ""
    Skype = ""
    Whatsapp = ""
    Zhihu = ""
    Douban = ""
    Angellist = ""
    Slidershare = ""
    Jsfiddle = ""
    Deviantart = ""
    Behance = ""
    Dribbble = ""
    Wordpress = ""
    Vine = ""
    Googlescholar = ""
    Researchgate = ""
    Mastodon = ""
    Thingiverse = ""
    Devto = ""
    Gitea = ""
    XMPP = ""
    Matrix = ""
    Bilibili = ""
    ORCID = ""
    Liberapay = ""
    Ko-Fi = ""
    BuyMeACoffee = ""
    Linktree = ""
    QQ = ""
    QQGroup = ""
    Email = "xxxx@xxxx.com"
    RSS = true # 

  #  文章页面配置
  [params.page]
    #  是否在主页隐藏一篇文章
    hiddenFromHomePage = false
    #  是否在搜索结果中隐藏一篇文章
    hiddenFromSearch = false
    #  是否使用 twemoji
    twemoji = false
    # 是否使用 lightgallery
    lightgallery = false
    #  是否使用 ruby 扩展语法
    ruby = true
    #  是否使用 fraction 扩展语法
    fraction = true
    #  是否使用 fontawesome 扩展语法
    fontawesome = true
    # 是否在文章页面显示原始 Markdown 文档链接
    linkToMarkdown = false
    #  配置编辑文章的链接
    linkToEdit = false
    # "https://github.com/user/repo/edit/main/{path}"
    # "https://gitlab.com/user/repo/-/edit/main/{path}"
    # "https://bitbucket.org/user/repo/src/main/{path}?mode=edit"
    #  是否在 RSS 中显示全文内容
    rssFullText = false
    #  页面样式 ("normal", "wide")
    pageStyle = "normal"
    #  是否在文章开头显示系列导航
    seriesNavigation = true
    #  过时文章提示
    [params.page.outdatedArticleReminder]
      enable = true
      # 如果文章最后更新于 90 天之前，显示提醒
      reminder = 90
      # 如果文章最后更新于 180 天之前，显示警告
      warning = 180
    #  目录配置
    [params.page.toc]
      # 是否使用目录
      enable = true
      #  是否保持使用文章前面的静态目录
      keepStatic = false
      # 是否使侧边目录自动折叠展开
      auto = false
    #  代码配置
    [params.page.code]
      # 是否显示代码块的复制按钮
      copy = false
      # 默认展开显示的代码行数
      maxShownLines = 10
    #  KaTeX 数学公式
    [params.page.math]
      enable = true
      # 默认块定界符是 $$ ... $$ 和 \\[ ... \\]
      blockLeftDelimiter = ""
      blockRightDelimiter = ""
      # 默认行内定界符是 $ ... $ 和 \\( ... \\)
      inlineLeftDelimiter = ""
      inlineRightDelimiter = ""
      # KaTeX 插件 copy_tex
      copyTex = true
      # KaTeX 插件 mhchem
      mhchem = true
    #  Mapbox GL JS 配置
    [params.page.mapbox]
      # Mapbox GL JS 的 access token
      accessToken = ""
      # 浅色主题的地图样式
      lightStyle = "mapbox://styles/mapbox/light-v9"
      # 深色主题的地图样式
      darkStyle = "mapbox://styles/mapbox/dark-v9"
      # 是否添加 NavigationControl
      navigation = true
      # 是否添加 GeolocateControl
      geolocate = true
      # 是否添加 ScaleControl
      scale = true
      # 是否添加 FullscreenControl
      fullscreen = true
    #  文章页面的分享信息设置
    [params.page.share]
      enable = false
      Twitter = true
      Facebook = true
      Linkedin = false
      Whatsapp = true
      Pinterest = false
      Tumblr = false
      HackerNews = false
      Reddit = false
      VK = false
      Buffer = false
      Xing = false
      Line = true
      Instapaper = false
      Pocket = false
      Digg = false
      Stumbleupon = false
      Flipboard = false
      Weibo = true
      Renren = false
      Myspace = true
      Blogger = true
      Baidu = false
      Odnoklassniki = false
      Evernote = true
      Skype = false
      Trello = false
      Mix = false
    #  评论系统设置
    [params.page.comment]
      enable = false
      # Disqus 评论系统设置
      [params.page.comment.disqus]
        # 
        enable = false
        # Disqus 的 shortname, 用来在文章中启用 Disqus 评论系统
        shortname = ""
      # Gitalk 评论系统设置
      [params.page.comment.gitalk]
        # 
        enable = false
        owner = ""
        repo = ""
        clientId = ""
        clientSecret = ""
      # Valine 评论系统设置
      [params.page.comment.valine]
        enable = false
        appId = ""
        appKey = ""
        placeholder = ""
        avatar = "mp"
        meta= ""
        pageSize = 10
        lang = ""
        visitor = true
        recordIP = true
        highlight = true
        enableQQ = false
        serverURLs = ""
        #  emoji 数据文件名称, 默认是 "google.yml"
        # ("apple.yml", "google.yml", "facebook.yml", "twitter.yml")
        # 位于 "themes/DoIt/assets/data/emoji/" 目录
        # 可以在你的项目下相同路径存放你自己的数据文件:
        # "assets/data/emoji/"
        emoji = ""
      # Waline 评论系统设置
      [params.page.comment.waline]
        # 
        enable = false
        serverURL = ""
        visitor = false
        emoji = ['https://cdn.jsdelivr.net/gh/walinejs/emojis/weibo']
        meta = ['nick', 'mail', 'link']
        requiredMeta = []
        login = 'enable'
        wordLimit = 0
        pageSize = 10
        uploadImage = false
        highlight = true
        mathTagSupport = false
        commentCount = true
      # Facebook 评论系统设置
      [params.page.comment.facebook]
        enable = false
        width = "100%"
        numPosts = 10
        appId = ""
        languageCode = "zh_CN"
      #  Telegram Comments 评论系统设置
      [params.page.comment.telegram]
        enable = false
        siteID = ""
        limit = 5
        height = ""
        color = ""
        colorful = true
        dislikes = false
        outlined = false
        dark = false
      #  Commento 评论系统设置
      [params.page.comment.commento]
        enable = false
      #  Utterances 评论系统设置
      [params.page.comment.utterances]
        enable = false
        # owner/repo
        repo = ""
        issueTerm = "pathname"
        label = ""
        lightTheme = "github-light"
        darkTheme = "github-dark"
      #  Twikoo 评论系统设置
      [params.page.comment.twikoo]
        enable = false
        envId = ""
        region = ""
        path = ""
        visitor = true
        commentCount = true
      #  Vssue 评论系统设置
      [params.page.comment.vssue]
        enable = false
        platform = "" # ("bitbucket", "gitea", "gitee", "github", "gitlab")
        owner = ""
        repo = ""
        clientId = ""
        clientSecret = ""
      #  Remark42 评论系统设置
      [params.page.comment.remark42]
        enable = false
        host = ""
        site_id = ""
        max_shown_comments = 15
        show_email_subscription = true
        simple_view = false
      #  giscus 评论系统设置
      [params.page.comment.giscus]
        enable = false
        # owner/repo
        dataRepo = ""
        dataRepoId = ""
        dataCategory = ""
        dataCategoryId = ""
        dataMapping = "pathname"
        dataReactionsEnabled = "1"
        dataEmitMetadata = "0"
        lightTheme = "light"
        darkTheme = "dark"
    #  第三方库配置
    [params.page.library]
      [params.page.library.css]
        # someCSS = "some.css"
        # 位于 "assets/"
        # 或者
        # someCSS = "https://cdn.example.com/some.css"
      [params.page.library.js]
        # someJavascript = "some.js"
        # 位于 "assets/"
        # 或者
        # someJavascript = "https://cdn.example.com/some.js"
    #  页面 SEO 配置
    [params.page.seo]
      # 图片 URL
      images = []
      # 出版者信息
      [params.page.seo.publisher]
        name = ""
        logoUrl = ""

  #  赞赏配置
  [params.sponsor]
    enable = false
    bio = "如果你觉得这篇文章对你有所帮助，欢迎赞赏~"
    link = "https://www.buymeacoffee.com" # 你的赞赏页面的地址
    custom = "" # 自定义 HTML 

  #  TypeIt 配置
  [params.typeit]
    # 每一步的打字速度 (单位是毫秒)
    speed = 100
    # 光标的闪烁速度 (单位是毫秒)
    cursorSpeed = 1000
    # 光标的字符 (支持 HTML 格式)
    cursorChar = "|"
    # 打字结束之后光标的持续时间 (单位是毫秒, "-1" 代表无限大)
    duration = -1

  # 网站验证代码, 用于 Google/Bing/Yandex/Pinterest/Baidu
  [params.verification]
    google = ""
    bing = ""
    yandex = ""
    pinterest = ""
    baidu = ""
    so = "" # 360 search
    sogou = ""

  #  网站 SEO 配置
  [params.seo]
    # 图片 URL
    image = ""
    # 缩略图 URL
    thumbnailUrl = ""

  #  网站分析配置
  [params.analytics]
    enable = false
    # Google Analytics
    [params.analytics.google]
      id = ""
      # 是否匿名化用户 IP
      anonymizeIP = true
    # Fathom Analytics
    [params.analytics.fathom]
      id = ""
      # 自行托管追踪器时的主机路径
      server = ""
    #  Baidu Analytics
    [params.analytics.baidu]
      id = ""
    #  Umami Analytics
    [params.analytics.umami]
      data_website_id = ""
      src = ""
      data_domains = ""
    #  Plausible Analytics
    [params.analytics.plausible]
      data_domain = ""
      src = ""

  #  Cookie 许可配置
  [params.cookieconsent]
    enable = true
    # 用于 Cookie 许可横幅的文本字符串
    [params.cookieconsent.content]
      message = ""
      dismiss = ""
      link = ""

  #  第三方库文件的 CDN 设置
  [params.cdn]
    # CDN 数据文件名称, 默认不启用
    # ("jsdelivr.yml")
    # 位于 "themes/DoIt/assets/data/cdn/" 目录
    # 可以在你的项目下相同路径存放你自己的数据文件:
    # "assets/data/cdn/"
    data = ""

  #  兼容性设置
  [params.compatibility]
    # 是否使用 Polyfill.io 来兼容旧式浏览器
    polyfill = false
    # 是否使用 object-fit-images 来兼容旧式浏览器
    objectFit = false

# Hugo 解析文档的配置
[markup]
  # 语法高亮设置 (https://gohugo.io/content-management/syntax-highlighting)
  [markup.highlight]
    codeFences = true
    guessSyntax = true
    lineNos = true
    lineNumbersInTable = true
    # false 是必要的设置
    # (https://github.com/dillonzq/LoveIt/issues/158)
    noClasses = false
  # Goldmark 是 Hugo 0.60 以来的默认 Markdown 解析库
  [markup.goldmark]
    [markup.goldmark.extensions]
      definitionList = true
      footnote = true
      linkify = true
      strikethrough = true
      table = true
      taskList = true
      typographer = true
    [markup.goldmark.renderer]
      # 是否在文档中直接使用 HTML 标签
      unsafe = true
  # 目录设置
  [markup.tableOfContents]
    startLevel = 3  # 起始等级设置很重要, 如果文章内的最高标签等级小于设定的标签等级, 会自动缩进, 目录结构会变得难看
    endLevel = 6

# 作者配置
[author]
  name = "wiz"
  email = ""
  link = ""
  avatar = ""
  gravatarEmail = ""

# 网站地图配置
[sitemap]
  changefreq = "weekly"
  filename = "sitemap.xml"
  priority = 0.5

# Permalinks 配置
[Permalinks]
  # posts = ":year/:month/:filename"
  posts = ":filename"

# 隐私信息配置
[privacy]
  #  Google Analytics 相关隐私 (被 params.analytics.google 替代)
  [privacy.googleAnalytics]
    # ...
  [privacy.twitter]
    enableDNT = true
  [privacy.youtube]
    privacyEnhanced = true

# 用于输出 Markdown 格式文档的设置
[mediaTypes]
  [mediaTypes."text/plain"]
    suffixes = ["md"]

# 用于输出 Markdown 格式文档的设置
[outputFormats.MarkDown]
  mediaType = "text/plain"
  isPlainText = true
  isHTML = false

# 用于 Hugo 输出文档的设置
[outputs]
  # 
  home = ["HTML", "RSS", "JSON"]
  page = ["HTML", "MarkDown"]
  section = ["HTML", "RSS"]
  taxonomy = ["HTML", "RSS"]
  taxonomyTerm = ["HTML"]

# 用于分类的设置
[taxonomies]
author = "authors"
category = "categories"
tag = "tags"
series = "series"

```


