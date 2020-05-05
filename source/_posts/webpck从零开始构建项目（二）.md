---
title: webpck从零开始构建项目之vue模板配置（二）
date: 2018-04-11 12:12:17
categories: webpack
tags:
  - webpack
  - 前端构建工具
---
# VUE页面成功访问

现在，为了解决服务端启动，但页面没有内容的问题
我们在配置文件中第二行引入webpack的打包插件

<!-- more -->

> const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

- html-webpack-plugin：HTML生成插件。

它会自动帮你生成一个 html 文件，并且引用相关的 assets 文件(如 css, js)

- clean-webpack-plugin：清理插件。

它会在在每次构建项目前，清理 /dist 文件夹

由于它们不是Node的内置方法，引入后，需要使用Npm安装对应插件

`npm install html-webpack-plugin clean-webpack-plugin -D`

介绍了插件，那就并且引入了项目，接下来自然就是使用了
在配置文件的四大基础配置项之插件项 `plugins` 中引入

    plugins: [
    +     new CleanWebpackPlugin(['dist']),
          new HtmlWebpackPlugin({
            title: 'Output Management'
          })
        ],

title是新建的HTML的标题，一切变得很容易理解了吧？

但要是就这样运行，还是没有什么卵用
这只是用于理解的官网示例代码。

不设置输入输出，你设置tltle也没什么用呐~

于是我们需要引入一个html文件，实际进行测试

    plugins: [
         new CleanWebpackPlugin(['dist']),
          new HtmlWebpackPlugin({
            template:'./app/views/index.html'
          })
        ],

html代码：

    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>自定义模板</title>
    </head>
    <body>
        <div id="app"></div>
    </body>
    </html>

即便如此，当你实际运行，你会发现还是没什么效果，顶多title变了，还是会报错

- 但你会发现，报错来自VUE

>Cannot find module 'vue-template-compiler'（找不到模块Vue的模板编译器）

这时候我们会百度，或者到vue官方文档

然后我们得到解决办法：这个问题很简单，没有编译器，安装就行了
`npm install vue-template-compiler --save-dev`

然后再次执行 `webpack-dev-server --open` 查看效果

我们发现还是有BUG：

> You are using the runtime-only build of Vue where the template compiler is not available. Either pre-compile the templates into render functions, or use the compiler-included build.

看起来很长，但是工整的报错最不用怕，我们在插件 `plugins `下面插入以下代码

    resolve: {
        alias: {
            'vue$': 'vue/dist/vue.esm.js'
        }
    },

最终效果：
![这里写图片描述](https://img-blog.csdn.net/20180411032526269?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2E1NDY1OTgxODU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

最后还有一个BUG，关于SCSS样式问题，浏览器可能会报错
这个时候根据报错提示npm安装node-sass就行

因为这个系列是从踩坑入门，所以对于处理报错，学到这里的同学应该也比较熟练了。
