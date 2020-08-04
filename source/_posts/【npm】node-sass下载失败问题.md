---
title: 【npm】node-sass下载失败问题
date: 2020-08-04 14:33:51
tags:
---

node-sass 因为一些历史原因，对 node 的版本有要求，必须要`node@8.x`以下才可以安装。

但平时我们不可能为了一个 npm 包，频繁切换 node 版本，尽管有 nvm 版本管理工具。

特别是在用到自动化平台，需要在云端进行打包工作的时候。

为了从手动操作中解放出来，其实我们可以使用`.npmrc` 配置文件对 `npm` 进行配置。

通过`.npmrc` ，我们可以通过观察报错信息，指定依赖包的下载地址。

```shell
#指定phantomjs下载地址
phantomjs_cdnurl=http://cnpmjs.org/downloads
#指定node-sass下载地址
sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
#指定chromedriver下载地址
chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver
#镜像源
registry=https://registry.npm.taobao.org
```
