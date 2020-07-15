---
title: 【nestjs】创建nestjs项目
date: 2020-05-14 01:22:27
tags:
  - nestjs
  - 教程笔记
---

Nest 是一个渐进的 Node.js 框架，可以在 TypeScript 和 JavaScript (ES6、ES7、ES8)之上构 建高效、可伸缩的企业级服务器端应用程序。

Nest 基于 TypeScript 编写并且结合了 OOP（面向对象编程），FP（函数式编程）和 FRP （函数式响应编程）的相关理念。在设计上的很多灵感来自于 Angular，Angular 的很多模 式又来自于 Java 中的 Spring 框架，依赖注入、面向切面编程等，所以我们也可以认为： Nest 是 Node.js 版的 Spring 框架。

Nest 框架底层 HTTP 平台默认是基于 Express 实现的，所以无需担心第三方库的缺失。

Nest 旨在成为一个与平台无关的框架。 通过平台，可以创建可重用的逻辑部件，开发人员可以利用这些部件来跨越多种不同类型的应用程序。

从技术上讲，Nest 可以在创建适配器 后使用任何 Node HTTP 框架。 有两个支持开箱即用的 HTTP 平台：express 和 fastify。 您 可以选择最适合您需求的产品。 NestJs 的核心思想：就是提供了一个层与层直接的耦合度极小,抽象化极高的一个架构 体系。

Nestjs 目前在行业内具有很高的关注度，所以我们有必要学习一下

官网：https://nestjs.com/

中文网站：https://docs.nestjs.cn/

GitHub: https://github.com/nestjs/nest

<!-- more -->

nest 主要是学习，在国内实际使用，还是优先选 eggjs（文档第一）

[TOC]

## 安装 nestjs

以下三种 nest 脚手架的全局安装方式，任选其一

> npm i -g @nestjs/cli

> yarn global add @nestjs/cli

> cnpm i -g @nestjs/cli

## 创建项目

> nest new nestdemo

接下来按提示，进入目录，运行即可
