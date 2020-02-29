---
title: 探索前端开发 - 搭建Vue开发环境
date: 2020-02-29 10:02:00
tags: ["Vue", "VSCode"]
categories: Vue前端项目框架
---

## 前言
纯前后端分离的项目结构，我们大多采用Vue前端 + asp.net core WebApi，本系列先从Vue前端开发开始。以前的项目采用的是Vue-Cli v2.x，最近发现已经发布了Vue-Cli v4.x，改动蛮大的。文章将以探索v3+的使用为主，进行使用经验的记录。

## 环境搭建
### Visual Studio Code
开发环境采用Visual Studio Code，这是宇宙无敌的免费开发环境，安装使用就像微软的所有产品一样，开箱即用，没有任何难度。传送门：[Visual Studio Code: https://code.visualstudio.com/](https://code.visualstudio.com/)

要在VSCode中开发，离不开插件的支持，对于Vue前端开发，推荐以下必装插件
* [Chinese (Simplified) Language Pack for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-zh-hans)
* [Vetur](https://marketplace.visualstudio.com/items?itemName=octref.vetur)
* [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
* [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)
<!-- more -->

### 中文语言包
对于英语不好的同学，这个插件安装后方便许多
### Vetur
Vue开发，必备插件，提供了语法高亮等功能
### Beautify
要写出格式统一，漂亮的代码，这个插件也是必不可少的。写完代码，格式化一下，省去很多烦恼。
### Debugger for Chrome
能在编辑器里直接调试Javascript代码，是完全不一样的体验，有了这个插件，就可以在原始代码文件中设置断点进行调试了。

## Vue-Cli 脚手架
### 安装
vue-cli 的官方站点 [Vue CLI](https://cli.vuejs.org/zh/)，文档非常完善。安装就是一个命令，当然，你要已经安装好了`Node.js`，才能执行`npm`命令
```
npm install -g @vue/cli
vue --version
```
我安装的是@vue/cli 4.2.2，Node.js v10.16.3
### 创建项目
使用vue-cli创建项目的命令是：
```
cd d:\study
vue create xxx-front
```
xxx-front是项目名称，可以按自己喜欢起名字。然后会出来一系列选项，我的选项如下：
* Manually select features
    * Babel
    * Progressive Web App (PWA) Support
    * Router
    * Vuex
    * CSS Pre-processors
    * Linter / Formatter
    * Unit Testing
* Use history mode for router? Y
* Sass/SCSS (with dart-sass)
* ESLint with error prevention only
    * Lint on save
* Mocha + Chai
* In dedicated config files

### 选项
![选项截图](/assets/images/vuefrontstep1/1.png)
### 完成
经过一段时间的下载和安装，结果如下：
![创建结果](/assets/images/vuefrontstep1/2.png)使用VS Code打开目录，目录结构如下：
![目录结构](/assets/images/vuefrontstep1/3.png)至此，一个Vue项目已经初始化完毕，项目依赖项也已经自动下载完成了。
## 运行项目
使用``Ctrl + ` ``快捷键打开终端窗口，输入`npm run serve`启动本地开发服务器
然后可以打开浏览器访问 `http://localhost:8080/` 查看效果了
### 调试配置
按`F5`启动调试，选择Chrome，会生成默认的`launch.json`调试配置文件，修改内容为以下内容：
```json
{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "vuejs: chrome",
            "url": "http://localhost:8080/",
            "webRoot": "${workspaceFolder}/src",
            "breakOnLoad": true,
            "sourceMapPathOverrides": {
                "webpack:///./src/*": "${webRoot}/*"
            }
        }
    ]
}
```
再次按F5启动调试，会打开一个Chrome并已经导航到 `http://localhost:8080/`