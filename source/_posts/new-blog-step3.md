---
title: 搭建 Hexo 博客系统
date: 2020-02-24 09:45:32
tags: ["Hexo", "NexT"]
---
## 前言
Github Pages默认使用的Jekyll生成博客，这个是要使用Ruby的，且中文资料实在太少。经过分析比较，还是Hexo更好一点，Hexo使用NodeJs，这个我有现成的。

## 环境搭建
Hexo的中文资料很多，参考官方的说明文档就够了，过程并不复杂，传送门：[https://hexo.io/zh-cn/](https://hexo.io/zh-cn/)
另外参考了以下文章
* [5分钟 搭建免费个人博客](https://www.jianshu.com/p/4eaddcbe4d12)
* [GitHub Actions自动部署Hexo博客](https://blog.motreen.ml/posts/35722/)

## 选择主题
NexT包含以下几种Scheme，在themes\\next\\_config.yml中配置
```
#scheme: Muse
#scheme: Mist
scheme: Pisces
#scheme: Gemini
```

## 迁移文章
复制原来的`_posts`和`assets`目录到`source`目录，修改了原文章中的章节导航