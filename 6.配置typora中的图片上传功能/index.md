# 使用文档 - 配置Typora中的图片上传功能


使用Typora+PicGo+Gitee搭建舒适便捷的个人写作环境

<!--more-->

### 安装PicGo

打开 Typora 依次点击: **文件**-->**偏好设置**-->**图像**, 进入到下图界面

{{< image src="https://pic.imgdb.cn/item/629a4968094754312954726d.png" caption="p1" width="80%" height="auto" >}}



点击**上传服务**后面的下拉框, 选择 **PicGo(app)** 选项, 然后点击下面的 **下载PicGo(app)** 按钮, 跳转到下载页面

{{< image src="https://pic.imgdb.cn/item/629a49680947543129547279.png" caption="p2" width="80%" height="auto" >}}



点击免费下载会跳到 GitHub, 如下图, 下拉到 **Assets**, 选择你需要的版本下载, 然后安装

{{< image src="https://pic.imgdb.cn/item/629a4968094754312954727f.png" caption="p3" width="82%" height="auto" >}}



### 安装插件

打开 PicGo, 点击**插件设置**, 搜索 gitee, 点击**安装**, 这时会提示安装 NodeJS, 根据提示下载, 安装后重启 PicGo, 再点击**安装**

{{< image src="https://pic.imgdb.cn/item/629a4968094754312954728b.png" caption="p4" width="80%" height="auto" >}}



这时就能在**图床设置**中看到**Gitee图床**

{{< image src="https://pic.imgdb.cn/item/629a49680947543129547299.png" caption="p5" width="80%" height="auto" >}}



### 建立连接

进入 Gitee 新建一个专门存储图片的仓库, 用户名和仓库名就是要填入PicGo 中的

{{< image src="https://pic.imgdb.cn/item/629a497509475431295483df.png" caption="p6" width="80%" height="auto" >}}



创建**私人令牌**, 依次点击右上角**头像**-->**设置**, 然后点击左边的**私人令牌**

{{< image src="https://pic.imgdb.cn/item/629a4993094754312954aa5f.png" caption="p7" width="80%" height="auto" >}}

进入下图页面

{{< image src="https://pic.imgdb.cn/item/629a497509475431295483e8.png" caption="p8" width="83%" height="auto" >}}



点击**生成新令牌**后进入下图页面, 如图选择权限, 输入描述信息后点击**提交**

{{< image src="https://pic.imgdb.cn/item/629a497509475431295483f0.png" caption="p9" width="55%" height="auto" >}}



生成新令牌后**立即复制**保存, 令牌不会再明文显示在平台上

{{< image src="https://pic.imgdb.cn/item/629a497509475431295483fc.png" caption="p10" width="55%" height="auto" >}}



打开 PicGo, 如下图填写, 点击**设为默认图床**

{{< image src="https://pic.imgdb.cn/item/629a49760947543129548405.png" caption="p11" width="80%" height="auto" >}}



### 上传图片

先设置一下 PicGo, 根据自己的习惯设置

{{< image src="https://pic.imgdb.cn/item/629a497d0947543129548e98.png" caption="p12" width="80%" height="auto" >}}



打开 Typora, 把图片拖进 Typora 时会有提示框, 在**上传图片**之前可以先设置一下要上传到哪个目录

{{< image src="https://pic.imgdb.cn/item/629a497d0947543129548e9d.png" caption="p13" width="60%" height="auto" >}}



设置上传到哪个目录: 打开 Picgo, 把下图的 path 改成你想存放的目录, 然后点击**确定**

{{< image src="https://pic.imgdb.cn/item/629a497d0947543129548ea4.png" caption="p14" width="80%" height="auto" >}}



然后在 Typora 中**右击**图片, 点击**上传图片**

{{< image src="https://pic.imgdb.cn/item/629a4993094754312954aa64.png" caption="p15" width="55%" height="auto" >}}



弹出重命名图片的窗口, 重命名后点击**确定**

{{< image src="https://pic.imgdb.cn/item/629a497d0947543129548eb5.png" caption="p16" width="60%" height="auto" >}}



这个时候你就可以在 Gitee 中的仓库中看到图片了

{{< image src="https://pic.imgdb.cn/item/629a497d0947543129548ebf.png" caption="p17" width="90%" height="auto" >}}








