---
title: 【docker】部署第一个应用doclever
date: 2020-05-07 00:43:22
tags:
---

[doclever 开源接口管理平台的 github 地址](https://github.com/sx1989827/DOClever/tree/master/docker)

为了方便部署和使用，我们使用 docker-compose

用法：

① 在文件夹下创建`docker-compose.yml`，填入以下内容

```
version: "2"
services:
  DOClever:
    image: lw96/doclever
    restart: always
    container_name: "DOClever"
    ports:
    - 20080:10000
    volumes:
    - /svr/doclever/file:/root/DOClever/data/file
    - /svr/doclever/img:/root/DOClever/data/img
    - /svr/doclever/tmp:/root/DOClever/data/tmp
    environment:
    - DB_HOST=mongodb://mongo:27017/DOClever
    - PORT=10000
    links:
    - mongo:mongo

  mongo:
    image: mongo:latest
    restart: always
    container_name: "mongodb"
    volumes:
    - /svr/doclever/db:/data/db
```

② 运行 `docker-compose up -d`
