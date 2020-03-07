---
title: 探索前端开发 - 2.完整项目框架
date: 2020-03-07 09:15:00
tags: ["Vue", "Element UI", "i18n", "多语言"]
categories: Vue前端项目框架
---

## 前言
目标是搭建一个可用的项目框架，将使用这些技术：`Element UI`用于布局和页面/组件功能，`i18n`实现多语言，`axios`进行Web API调用。先放一张最终效果图：
![最终效果](/assets/images/vuefrontstep2/1.png)

## 多语言
Vue i18n 的官方站点 [http://kazupon.github.io/vue-i18n/zh/](http://kazupon.github.io/vue-i18n/zh/)，文档非常完善。安装就是一个命令，`vue add i18n`，4个问题的选项如下：
```
? The locale of project localization. zh
? The fallback locale of project localization. zh
? The directory where store localization messages of project. It's stored under `src` directory. locales
? Enable locale messages in Single file components ? No
```
安装后，项目会多一些文件，多语言文件在`src\locales\zh.json`，自行增加一个`en.json`，使用方法参见官方文档，后面等ElementUI加上后，再补充如何切换语言。

## 页面布局
### 安装 Element UI
Element UI 的官方站点 [https://element.eleme.cn/2.13/#/zh-CN](https://element.eleme.cn/2.13/#/zh-CN)，文档非常完善。vue-cli 3.0+的插件地址 [vue-cli-plugin-element](https://github.com/ElementUI/vue-cli-plugin-element)，一条命令进行安装：`vue add element`，问题选项建议如下：
```
? How do you want to import Element? Fully import
? Do you wish to overwrite Element's SCSS variables? Yes
? Choose the locale you want to load zh-CN
```
安装后，项目会多一些文件，重要的是`element-variables.scss`文件，样式的覆盖建议写在这个文件里。
> 特别强调，插件式安装会修改`App.vue`，如果是后期加入element的，请注意保留好源文件。
强烈建议，本地单人开发也使用git来管理源代码

### 站点入口 App.vue
修改`App.vue`文件，内容如下：
```html
<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  height: 100%;
}
</style>
```
### 主布局入口 pcMain.vue
在`views`目录下增加`pcMain.vue`文件，内容如下：
```html
<template>
  <el-container class="app" direction="vertical">
    <Header />
    <el-container>
      <LeftMenu width="200px" />
      <el-main class="middle">
        <div class="breadcrumb">
          <i class="el-icon-s-promotion" style="color:#1890ff"></i>
          <el-breadcrumb separator="/">
            <el-breadcrumb-item>{{$t("appName")}}</el-breadcrumb-item>
            <el-breadcrumb-item>{{$t("menu." + $route.name)}}</el-breadcrumb-item>
          </el-breadcrumb>
        </div>
        <router-view></router-view>
      </el-main>
    </el-container>
    <Footer />
  </el-container>
</template>

<script>
import Header from "@/components/Header.vue";
import LeftMenu from "@/components/LeftMenu.vue";
import Footer from "@/components/Footer.vue";
export default {
  name: "Home",
  components: {
    Header,
    LeftMenu,
    Footer
  }
};
</script>
```
### 路由表 index.js
修改路由表`router\index.js`文件，修改`/`，增加`/pcMain`
```
  {
    path: '/',
    redirect: '/pcMain'
  },
  {
    path: '/pcMain',
    name: 'pcMain',
    component: resolve => require(['../views/pcMain'], resolve),
    redirect: '/pcMain/home',
    children: [{
        path: 'home',
        name: 'Home',
        component: Home
      }
    ]
  }
```
### 全局配置 config.js
在`public`目录下新建`config.js`文件，内容如下：
```js
const AppConfig = {
    "TenantName": "Demo",
    "ApiBaseUrl": "https://localhost:44382/api",
    "Environment": "Development",
    "AppVersion": "20200216.1"
}
```
> 独立出这些文件，主要是用于CI/CD等需要在不同环境不同配置，public下的文件名固定，不会被webpack打包为随机文件名
需要在`.eslintrc.js`文件内增加以下内容，不然会有编译错误
```
  globals: {
    AppConfig: false
  },
```
在`public\index.html`文件的`head`区增加`<script src="/config.js"></script>`
### 顶部底部和菜单
在`components`目录下依次增加`Header.vue`，`Footer.vue`，`LeftMenu.vue`文件
```html
<template>
  <el-header class="header">
    <div class="logo">
      <router-link to="/">
        <img alt="Demo System" src="../assets/logo.png" />
        <h1>{{AppConfig.TenantName}}</h1>
      </router-link>
    </div>
    <el-menu :default-active="activeIndex" class="topMenu" mode="horizontal" router>
      <el-menu-item index="/pcMain">主模块</el-menu-item>
      <el-menu-item index="/about">关于</el-menu-item>
    </el-menu>
    <div class="topMenu"></div>
    <el-dropdown trigger="click" class="locale-changer" @command="changeLocale">
      <span class="el-dropdown-link">
        {{ $t('lang')}}
        <i class="el-icon-arrow-down el-icon--right"></i>
      </span>
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item
          v-for="(item, i) in langs"
          :key="`Lang${i}`"
          :command="item.lang"
        >{{ item.label }}</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>
  </el-header>
</template>

<script>
export default {
  name: "Header",
  data() {
    return {
      langs: [
        { lang: "zh", label: "简体中文" },
        { lang: "en", label: "English" }
      ],
      AppConfig: AppConfig
    };
  },
  computed: {
    activeIndex() {
      return this.$route.matched[0].path;
    }
  },
  methods: {
    changeLocale(command) {
      this.$i18n.locale = command;
    }
  }
};
</script>
```
```html
<template>
  <el-footer class="footer" height="40px">
    <div class="left">
      Demo Copyright © 2019 - {{new Date().getFullYear()}}
      <span>Version {{AppConfig.AppVersion}}</span>
    </div>
    <div class="right">
      <img src="../assets/logo.png" class="logo-footer" />
      XXX公司 运维
    </div>
  </el-footer>
</template>

<script>
export default {
  name: "Footer",
  data() {
    return {
      AppConfig: AppConfig
    };
  }
};
</script>
```
```html
<template>
  <el-aside :width="menuWidth">
    <el-menu
      class="leftMenu"
      :default-active="choosePath"
      :collapse="isCollapse"
      unique-opened
      router
    >
      <el-menu-item
        v-for="(item,index) in menu"
        :key="(item, index)"
        :index="item.path"
      >
        <i :class="item.icon"></i>
        <span slot="title">{{item.name}}</span>
      </el-menu-item>
      <el-menu-item @click="collapseMenu" style="text-align:center">{{isCollapse?"&gt;":"&lt;"}}</el-menu-item>
    </el-menu>
  </el-aside>
</template>

<script>
export default {
  name: "LeftMenu",
  props: {
    width: String
  },
  data() {
    return {
      isCollapse: false,
      menu: [
        {
          name: "首页",
          path: "/pcMain/home",
          icon: "el-icon-house",
        },
        {
          name: "用户管理",
          path: "/pcMain/userManage",
          icon: "el-icon-user"
        },
        {
          name: "菜单2",
          path: "/pcMain/userManage",
          icon: "el-icon-user"
        }
      ]
    };
  },
  computed: {
    choosePath() {
      return this.$route.path;
    },
    menuWidth() {
      return this.isCollapse? "65px": this.width;
    }
  },
  methods: {
    collapseMenu() {
      this.isCollapse = !this.isCollapse;
    }
  }
};
</script>
```
修改多语言文件`zh.json`
```json
{
  "welcome": "欢迎使用",
  "lang": "选择语言",
  "appName": "示例系统",
  "menu": {
    "Home": "首页",
    "UserSetting": "个人信息",
    "UserManage": "用户管理"
  }
}
```
### 样式表 element-variables.scss
```scss
html,
body {
    margin: 0;
    padding: 0;
    height: 100%;
}

.el-container.app {
    min-height: 100vh;

    .el-header.header {
        border-bottom: 1px solid #e6e6e6;
        display: flex;

        a {
            text-decoration: none;
        }

        .logo {
            width: 200px;
            box-sizing: border-box;


            img {
                margin: 15px auto;
                height: 30px;
                display: block;
                float: left;
            }

            h1 {
                margin-top: 10px;
                height: 40px;
                line-height: 40px;
                color: rgb(0, 120, 212);
            }
        }

        .topMenu {
            flex: 0.5;
        }

        .locale-changer {
            line-height: 30px;
            height: 30px;
            margin-top: 15px;
            cursor: pointer;
            margin-right: 20px;
        }

        .avatarmenu {
            line-height: 30px;
            height: 30px;
            margin-top: 15px;

            .el-dropdown-link {
                cursor: pointer;

                .avatar {
                    width: 30px;
                    height: 30px;
                    vertical-align: top;
                    margin-right: 10px;
                }

                span {
                    display: inline-block;
                }
            }
        }
    }

    .el-aside {

        .leftMenu {
            min-height: 100%;
        }
    }

    .el-main.middle {
        background-color: whitesmoke;

        .breadcrumb {
            display: flex;
            margin: -20px -20px 10px -20px;
            background-color: white;

            i {
                line-height: 30px;
                margin: 0px 10px;
            }

            .el-breadcrumb {
                height: 30px;
                line-height: 30px;
            }
        }

        .el-row {
            margin-top: 20px;
        }
    }

    .el-footer.footer {
        line-height: 38px;
        border-top: 1px solid #e6e6e6;
        font-size: 12px;
        background: #fff;
        color: #909399;

        .left {
            float: left;
        }

        .right {
            float: right;
        }

        .logo-footer {
            height: 16px;
            margin-top: 12px;
            vertical-align: top;
        }
    }
}
```
## 预览效果
![预览效果](/assets/images/vuefrontstep2/2.png)
