---
title: 【docker】centos安装docker和docker-compose
date: 2020-05-07 00:21:25
tags:
  - docker
  - docker-compose
  - centos
---

[TOC]

[docker 官方文档：centos 安装](https://docs.docker.com/engine/install/centos/)

## docker

### 一、先运行卸载，确保环境安全

```
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

### 二、使用储存库安装

在首次在新主机上安装 Docker 引擎之前，需要设置 Docker 存储库。之后，您可以从存储库中安装和更新 Docker。

设置存储库需要安装`yum-utils`包

它提供了`yum-config-manager`工具，并设置稳定储存库。

```
sudo yum install -y yum-utils
```

```
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

### 三、安装 docker

```
# 安装最新版
sudo yum install docker-ce docker-ce-cli containerd.io
```

```
# 安装指定版
# 查看版本列表
yum list docker-ce --showduplicates | sort -r

# 指定版本 有些版本有冒号
# 例如docker-ce.x86_64  3:18.09.1-3.el7
# 应该忽略冒号之前的内容，正确示例：docker-ce-18.09.1
sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
```

### 四、启动 docker

> sudo systemctl start docker

### 五、试运行 hello-world

> sudo docker run hello-world
> 运行成功则安装成功，且能正常使用

## docker-compose

参考菜鸟教程：https://www.runoob.com/docker/docker-compose.html

Linux 上我们可以从 Github 上下载它的二进制包来使用，最新发行的版本地址：https://github.com/docker/compose/releases。

运行以下命令以下载 Docker Compose 的指定版本 1.25.4，要安装其他版本的 Compose，请替换 1.24.1。

```
curl -L https://github.com/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```

将可执行权限应用于二进制文件：

```
chmod +x /usr/local/bin/docker-compose
```

创建软链：

```
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

测试是否安装成功

```
docker-compose --version

# 成功打印 docker-compose version 1.25.4, build 8d51620a
```

### 配置加速地址

道客云：http://get.daocloud.io/

道客云提供加速：https://www.daocloud.io/mirror

运行加速配置

> curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://f1361db2.m.daocloud.io
