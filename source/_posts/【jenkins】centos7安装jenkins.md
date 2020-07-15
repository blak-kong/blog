---
title: 【jenkins】centos7安装jenkins
date: 2020-05-29 01:16:45
tags:
  - k8s
  - 教程笔记
---

[toc]

# 1.准备工作/linux 环境

## 1-1 配置 yum 源

不建议使用 CentOS 7 自带的 yum 源，因为安装软件和依赖时会非常慢甚至超时失败。这里，我们使用阿里云的源予以替换，执行如下命令，替换文件

`/etc/yum.repos.d/CentOS-Base.repo`

```linux
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

yum makecache
```

<!-- more -->

## 1-2 关闭防火墙

防火墙一定要提前关闭，否则在后续安装 K8S 集群的时候是个 trouble maker。执行下面语句关闭，并禁用开机启动：

```linux
[root@localhost ~]# systemctl stop firewalld & systemctl disable firewalld
[1] 10341
Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.
Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
```

## 1-3 关闭 Swap

类似 ElasticSearch 集群，在安装 K8S 集群时，Linux 的 Swap 内存交换机制是一定要关闭的，否则会因为内存交换而影响性能以及稳定性。这里，我们可以提前进行设置：

执行 swapoff -a 可临时关闭，但系统重启后恢复
编辑`/etc/fstab`，注释掉包含 swap 的那一行即可，重启后可永久关闭，如下所示：

```linux
/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=20ca01ff-c5eb-47bc-99a0-6527b8cb246e /boot                   xfs     defaults        0 0
# /dev/mapper/centos-swap swap
```

或直接执行(这里推荐使用这个，比较方便):

```linux
sed -i '/ swap / s/^/#/' /etc/fstab
```

关闭成功后，使用 top 命令查看，如下图所示表示正常：
![](https://upload-images.jianshu.io/upload_images/6709127-d57398a8c3b2ed63.png?imageMogr2/auto-orient/strip|imageView2/2/w/843/format/webp)

# 2.docker 安装

jenkins 是 java 开发的，本人不懂 java，同时只是想要简单使用，所以采用 docker 安装。

这里，我们使用 yum 方式安装 Docker 社区最新版。

Docker 官方文档是最好的教材：
https://docs.docker.com/install/linux/docker-ce/centos/#prerequisites

但由于方教授的防火墙，文档网站经常无法查看，并且使用 yum 安装也经常会超时失败。我们使用如下方式解决：

## 2-1 添加仓库

添加阿里云的 Docker 仓库：

```linux
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

yum makecache
```

## 2-2 安装 Docker

执行以下命令，安装最新版 Docker：

```linux
yum install docker-ce -y
```

安装成功后，如下图所示：

![](https://upload-images.jianshu.io/upload_images/6709127-b5ed7d66818401f8.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

运行 docker --version,可以看到安装了截止目前最新的 18.03.1 版本：

```linux
[root@localhost ~]# docker --version
Docker version 18.03.1-ce, build 9ee9f40
```

## 2-3 启动 Docker

启动 Docker 服务并激活开机启动：

```liunx
systemctl start docker & systemctl enable docker
```

运行一条命令验证一下：

```linux
docker run hello-world
```

# 3.jenkins 安装

# 4.jenkins 配置

# 5.参考来源

从零开始搭建 Kubernetes 集群 - 简书 https://www.jianshu.com/p/78a5afd0c597 (docker 与 linux 部分参考)
