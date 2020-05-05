---
title: 【HTTPS】记一次阿里云配置https
date: 2020-05-05 21:02:03
categories:
  - 云服务器
tags:
  - https
  - nginx
---

[TOC]

给阿里云配置 https 很简单，只需要申请证书、然后安装到云服务器内的 web 服务器中，配置一下即可。

阿里云有官方案例，这里选择 nginx

https://help.aliyun.com/document_detail/98728.html?spm=a2c4g.11186623.6.587.84fd392cMbk2oO

## nginx 安装步骤

我的 nginx 是使用 yum 安装的，所以安装目录在`/etc/nginx`

> yum install -y nginx

强烈推荐使用这种方式安装，非常方便

### 第一步：上传密钥到 nginx 目录下

在 nginx 目录下运行

> mkdir cert

创建密钥文件夹，把密钥文件都上传到该文件夹下。

### 第二步：配置 nginx

我们使用 nginx 不应该任何时候都直接改动 `nginx.conf`，这里我有一个`vhost/blog.conf`的文件夹，里面配置了一个代理的网站。参考配置

```nginx
server {
  listen 80;
  root /home/www/blog-website;
  server_name www.lzwlook.fun lzwlook.fun;
  rewrite ^/(.*)$ https://www.lzwlook.fun/$1 permanent;
  location / {
  }
}
server {
  listen 443 ssl;
  root /home/www/blog-website;
  server_name www.lzwlook.fun lzwlook.fun;

  ssl_certificate /etc/nginx/cert/3847437_www.lzwlook.fun.pem;
  ssl_certificate_key /etc/nginx/cert/3847437_www.lzwlook.fun.key;
  ssl_session_cache shared:SSL:1m;
  ssl_session_timeout  10m;
  ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;  #使用此加密套件。
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;   #使用该协议进行配置。
  ssl_prefer_server_ciphers on;
  location / {
  }
}
```

### 三、重启 nginx

```
nginx -s reload
```

### 补充：使用 include 管理项目

目前对于 nginx 不够了解，仅仅是文抄公。

这里再补一下，创建 vhost 文件夹后，nginx 中怎么生效。

我们还需要在 nginx.conf 中通过`include`，进行引入

```nginx
http {
    include /etc/nginx/conf.d/*.conf; # 这个是自带的
    include /etc/nginx/vhost/*.conf; # 这是我们的配置
}
```

在 nginx.conf 文件中使用 include 引入文件夹和配置，会让项目的管理方便很多，这样只要给每个新增项目建一个 `\*\*.conf` 文件就好了。
