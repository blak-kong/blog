---
title: webpack从零开始构建项目之rem问题（三）
date: 2018-04-11 14:07:12
categories:
  - webpack
  - rem
tags:
  - webpack
  - 前端构建工具
  - rem
---

[TOC]

# 使用webpack解决px转rem的自适应问题

现在的项目配置已经可以正常运行
接下来我们要考虑下一个问题，由于VUE主要适用于移动端，所以我们需要一个自适应方案
由于这是一个大众需求的刚需方案，所以我们大可不必自己手动去写JS代码，重复造轮子

<!-- more -->

那么我们要如何解决这个问题呢？

和前面使用过的各种loader一样，我们可以通过装载一个loader来实现功能

然后只是装载还不行，因为要使用到VUE里，我们还需要手动配置一下。
所以现在去VUE的官方文档查询→生产环境配置，找到VUE在webpack如何设置预处理器
（px转rem也属于CSS预处理）

然后了解大概语法后，我们就可以在Npmjs.com搜索与Px、rem相关的loader

本次教程使用的是px2rem-loader
在配置文件中修改以下代码

    {
        test: /\.vue$/,
        loader: 'vue-loader',
        options:{
            loaders:{
                css: 'vue-style-loader!css-loader!px2rem-loader?remUnit=75&remPrecision=8',
                scss: 'vue-style-loader!css-loader!px2rem-loader?remUnit=75&remPrecision=8!sass-loader'
            }
        }
    }

效果图：

![这里写图片描述](https://img-blog.csdn.net/20180411135910116?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2E1NDY1OTgxODU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

然后我们重新启动webpack-dev-server,打开网页，刷新页面看一下，元素的px是否已经转化成rem
上面的代码中，你可能会有不懂的地方，但我不会直接写出来告诉你
请自行前往npm搜索到对应插件，然后进入开源作者的github中查询，动手加深记忆
