---
title: webpack从零开始构建项目之启动配置（一）
date: 2018-04-11 11:59:37
categories: webpack
tags:
  - webpack
  - 前端构建工具
---
[TOC]

# webpack学习：构建工具详解

## 建议结合官方文档查看此系列

#手动搭建一个项目

## 项目准备

1. 创建目录

2. 初始化

   npm init → 创建package.json

   <!-- more -->

3. 创建业务目录

   app -> js -> main,APP.vue

   app -> css -> reset.scss

   app -> views -> index.html


注意：本系列博客使用的版本是
webpack3.10.0，
"vue": "^2.5.16"，
"vue-router": "^3.0.1"，
"webpack-dev-server": "^2.9.5"


再补上一个目录结构

![这里写图片描述](https://img-blog.csdn.net/20180411032351819?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2E1NDY1OTgxODU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这里出了一个错，views文件夹要放到js文件夹里

## 创建配置文件

1. 创建配置文件

   webpack.config.js

2. 文件配置

	  基础配置 ：     entry（入口）—— module（模块）——plugins（插件）—— output（输出）


      进阶配置：    resolve（解析器）——devtool（开发工具）——devServer（开发服务器）


---

在webpack.config.js输入以下代码，设置基础模块、以及输入输出打包功能

---

	// 当成模块输出
	module.exports = {
    // 入口
	    entry: {
        app: './app/js/main.js'
    },
    // 开发服务器
    devServer: {
        // 静态文件打包出口
        contentBase: path.join(__dirname, "dist"),
        // 启用gzip 压缩
        compress: true,
        // 端口
        port: 9000
    },
    // 模块
    module: {
        loaders: [{
            test: /\.html$/,
            loader: 'html-loader'
        }, {
            test: /\.vue$/,
            loader: 'vue-loader'
        }, {
            test: /\.scss$/,
            loader: 'style-loader!css-loader!sass-loader'
        }]
    },
    // 插件
    plugins: [],
    // 输出
	   output: {
        // 打包后的输出的文件名
        filename: '[name].min.js',
        // 打包后的文件存放的地方
		path: path.resolve(__dirname, 'dist')
    }
}`

---

   ## 进阶配置-网络配置devServer

   根据需要设置
   设置配置时，如有疑问， 官方文档查询最为快捷

   ---


	   devServer: {
       contentBase: path.join(__dirname, "dist"), // 静态文件输出地址
       compress: true, // 一切服务都启用gzip 压缩
       port: 9000 // 端口号
	   }

	   // 把上面的代码加入以下代码后面
	   module.exports = {
       entry: {
           app: './app/js/main.js'
       },



   - 这之后，执行命令 `npm install webpack-dev-server --save-dev`

	安装本地网络服务

   webpack-dev-server 为你提供了一个服务器和实时重载（live reloading） 功能。

   然后使用Npm继续安装module下,loaders里面的Loader。

 `npm install html-loader vue-loader style-loader css-loader sass-loader -D`

## 启动的代码配置

VUE是组件化的框架，所以首先要到组件的文件夹，创建用于显示的组件。
在home文件夹的index.vue中输入代码

---

	<template>
	  <div class="home">
      <h1>home</h1>
	  </div>
	</template>
	<script>
	export default {
	}
	</script>
	<style lang="scss">
	.home{
	color:blue;
    font-size: 80px;
	}
	</style>

---

然后为了显示组件，我们需要设置路由，把组件绑定到主页。
来到router目录，找到index.js文件，输入以下代码

---

	import Vue from "vue";
	import Router from "vue-router";
	import Home from "../home/index.vue";

	Vue.use(Router);

	export default new Router({
	    routes:[{
	        path:'/',
	        name:'home',
	        component:Home
	    }]
	})

---

此时组件已经绑定到了主页，但我们还没有编写vue的真正主页，App.vue的代码
所以我们需要到App.vue编写代码

---

	<template>
	  <div id="app">
	      <router-view></router-view> // 路由中绑定的页面
	  </div>
	</template>
	<script>
	export default {
	  name: "app"
	}
	</script>
	<style lang="scss">
	</style>

---

那么这个时候我们是不是就可以启动页面了呢？

答案是否定的。

由于是手动创建配置文件，其实目前为止，我们并没有在项目文件夹里面安装vue和vue-router，webpack也没有安装。

所以我们还需要执行`npm install vue vue-router`  和  `npm install webpack -D`

然后

再配置一下入口的mian.js

	import Vue from "vue"
	import App from "./App.vue"
	import router from "./router"

	Vue.config.productionTip = false

	/* eslint-disable no-new */
	new Vue({
	    el: "#app",
	    router,
	    components: { App },
	    template: "<App/>",
	})
	/* eslint-enable no-new */

因为我们刚才安装了本地网络服务，所以可以输入根据官方文档提供的使用信息，打开命令行

执行 `webpack-dev-server --open`

这时候就可以进去了。

但……还是会报错！

（继续改bug，请阅读下一章）
