# 使用文档 - 使用gitee作为图片仓库


把图片存到gitee上, 作为图片仓库

<!--more-->

### 1 在gitee新建仓库

打开 https://gitee.com/ , 创建一个仓库

### 2 克隆到本地

```bash
git clone https://gitee.com/willrealize/images.git
```

### 3 推送

```bash
# 把图片复制到本地仓库, 推送到gitee
git add .
git commit -m "添加图片"
git push
```

### 4 使用图片

下面是gitee上面的图片地址, 不能直接使用:

`https://gitee.com/willrealize/images/blob/master/hugo_images/skill/gitalk08.png`

把blob修改为raw就可以使用了:

`https://gitee.com/willrealize/images/raw/master/hugo_images/skill/gitalk08.png`

地址为下面格式:

`https://gitee.com/用户名/仓库名/raw/master/目录名/图片.图片格式`


