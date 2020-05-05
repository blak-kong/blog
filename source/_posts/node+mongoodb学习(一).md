---
title: node+mongoodb学习
date: 2019-04-07 20:26:53
categories: 前端
tags:
  - node
  - mongoodb
  - 实战笔记
---

[toc]

#### 说明
此系列将在一周内更新完，不记细节，只记基础知识点

#### 1.服务器启动
Express 是一个简洁而灵活的 node.js Web应用框架, 提供了一系列强大特性帮助你创建各种 Web 应用，和丰富的 HTTP 工具。

使用 Express 可以快速地搭建一个完整功能的网站。

<!-- more -->

Express 框架核心特性：

- 可以设置中间件来响应 HTTP 请求。
- 定义了路由表用于执行不同的 HTTP 请求动作。
- 可以通过向模板传递参数来动态渲染 HTML 页面。

启动文件：server.js
```javascript
const express = require("express");const express = require("express");
const app = express(); // 实例化
const port = process.env.PORT || 5000; // 端口号

app.get("/", (req, res) => {
    res.send("hello world!") // 默认输出
})
app.listen(port, () => { // 监听
    console.log(`Server running on port ${port}`);
})
```
此时推荐安装插件nodemon，它默认会监听当前目录，重启Express

#### 2.mongodb创建数据库
数据库：config/keys.js
```javascript
module.exports = {
    // 本地写法，无需登录
    mongoURI: "mongodb://localhost:27017/testData"
}

module.exports = {
    // 远程写法
    mongoURI: "mongodb://名称:密码@远程 数据库地址/testData"
}
```

回到启动文件：server.js
```javascript
const mongoose = require("mongoose") // 引入mongo

// DB config
const db = require("./config/keys").mongoURI; // 引入数据库地址

// connect to mongodb 链接mongodb
mongoose.connect(db, {useNewUrlParser: true})
        .then(() => console.log("MongoDB Connected 666")
        .catch(err => console.log(err))
```

访问mongodb成功后，node命令行会输出MongoDB Connected 666

#### 3.创建接口

接口目录:router/api/user.js
以下为最简单的router接口访问，只能输出消息。
```javascript
// login & register
const express = require("express");
const router = express.Router();


// $route   GET api/users/test
// @desc    返回的请求的json数据
// @access  public
router.get("/test", (req, res) => {
    res.json({msg: "login works"})
})
```
如果需要实现更多功能，则需要编写逻辑，甚至安装更多插件配合。
例：
```javascript
const bcrypt = require("bcrypt"); // 加密
const gravatar = require('gravatar'); // 全球公共头像
const jwt = require("jsonwebtoken"); // 生成token
```

#### 4.为什么接口要用到Schema
在数据库中，schema（模式）是数据库的组织和结构。
也就是数据结构。

1.在创建接口后，根据字段需要，创建schema（模式）。

2.该接口在前后端的字段传值，必须符合schema（模式）的定义。

例：
```javascript
const mongoose = require("mongoose")
const Schema = mongoose.Schema;

// Create Schema 模型
const UserSchema = new Schema({
    name: {
        type: String,
        required: true
    },
    email: {
        type: String,
        required: true
    },
    password: {
        type: String,
        required: true
    },
    avatar: {
        type: String
    },
    date: {
        type: Date,
        default: Date.now
    }
})

module.exports = User = mongoose.model("users", UserSchema);
```
#### 5.接口调试工具：postman
1.谷歌应用商店搜索postman，安装使用

2.直接百度搜索，下载安装软件

#### 5.1.Express中间件body-parser
当express使用get以外的请求时，需要安装中间件body-parser处理不同类型的请求

body-parser实现的·要点如下：

- 处理不同类型的请求体：`比如text、json、urlencoded等，对应的报文主体的格式不同`。
- 处理不同的编码：`比如utf8、gbk等`。
- 处理不同的压缩类型：`比如gzip、deflare等`。
- 其他边界、异常的处理。
