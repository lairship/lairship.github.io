---
layout: post
title:  "博客建站第二篇 - 选择博客主题"
date: 2020-02-23 15:45:32
tags: github
---

## 选择博客主题
在这一篇里，我们将学习如何选择主题，并写下第二篇博客。

1. 进入仓库的Settings
![Settings](/assets/images/newblogstep2/1.png)

1. 点击`Choose a theme`
![Choose a theme](/assets/images/newblogstep2/2.png)

1. 选择minimal，并点击`Select theme`
![Select theme](/assets/images/newblogstep2/3.png)

等待片刻，github会自动帮你增加一个配置文件`_config.yml`到仓库根目录，并帮你自动生成新博客，预览效果如下：
![Preview](/assets/images/newblogstep2/4.png)

## 写下第二篇博客
然后到仓库的根目录，使用`Create new file`按钮

1. 输入博客地址`_posts/202-02-23-new-blog-step2.md`
    > 博客需要放在`_posts`目录下，并且文件的格式要求为：`yyyy-MM-dd-title.md`

1. 输入博客内容，格式为
    ```
    ---
    layout: post
    title:  "博客标题"
    ---

    # 博客标题

    博客内容
    ```

1. 点击`Commit new file`，提交博客内容
![New Blog](/assets/images/newblogstep2/5.png)

## 再次切换主题
然后去查看博客站点，发现找不到地方查看第二篇博客。再次研究了一下主题，发现通过选择主题界面选择的那个主题是MINIMAL，在`_config.yml`中的内容为
```
theme: jekyll-theme-minimal
```
这个主题比较简单，默认显示不了其它博客，修改为Jekyll的默认主题minima(注意结尾少一个l字母，我就是没注意看，搞了半天)，直接修改`_config.yml`文件内容为：
```
title: lairship's blog
author: lairship
theme: minima
```

再次查看博客站点，在首页的末尾有了第二篇博客的入口，可以查看了。

## 收尾
另外，一般情况下，README.md应该作为首页的，因此把第一篇博客的地址可以移动`_posts`目录下，同时修改README.md的内容为一些简介概览。

补充一个截图如下：
![New Preview](/assets/images/newblogstep2/6.png)