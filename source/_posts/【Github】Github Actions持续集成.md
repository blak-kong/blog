---
title: 【Github】Github Actions持续集成
date: 2020-07-24 00:05:52
tags:
 - Github Actions
 - Github
---

2020/7/23 在掘金看到一篇用 github 打造个人简历的文章，骚操作顿时令我惊为天人。没忍住，在待业期间不务正业，花了一天时间各种踩坑，最后经过整理，有了这篇文章。

但这篇文章也并不是教程，最好的教程仍旧是官方文档，本文主要是起到一个抛砖引玉的作用！简单聊聊自己是如何学习使用的！

对于Github Actions 持续集成的整体介绍、学习范围、能达成的目标，这最重要的入门阶段，推荐阮一峰的教程：

GitHub Actions 入门教程 - 阮一峰的网络日志 http://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html

## Github Actions 如何开始使用？

即便已经看过入门教程，并提交 demo 完成了体验。对于一些初级、或者没有后端经验的同学，仍旧可能是懵逼状态。

因为经验上的问题，仅仅使用可能无法充分的理解部分操作流程背后的设计思想。

同时由于**知识的诅咒（当一个人知道一件事后，他就无法想象自己是不知道这件事的）**，注定了知识在传播的过程中，总是存在着一定的信息差。

试图完全抹平这份差距是愚蠢的。例如 shell 脚本，部分初级的同学可能甚至没有了解过，就会比较难受，但也并不是无法学习使用。

## actions

### 1-1 什么是 actions？

actions：是一个定义好的工作流脚本。开发者们可以把一段脚本的工作流抽象出来，形成动作（action）。开源到GitHub后，这段脚本就会如同组件一般，可以在自己的工作流中引入，直接进行配置、使用。

### 1-2 如何合理高效的检索/使用 actions？

GitHub 做了一个官方的 actions 插件市场，入口为 Github 首页顶部栏的 marketplace（市场），通过它可以搜索到他人提交的 action。

另外，还有一个 [sdras/awesome-actions](https://github.com/sdras/awesome-actions) 的仓库，可以找到很多优秀 action。

sdras 是vuejs的核心团队成员，我们可以根据各自需要，直接学习使用她分类整理的 actions 索引列表。

对于不同 action 的使用，我们进入对应的仓库，都会有使用示例。

### 1-3 我是有极客精神的精神小伙，我能创建自己的 action 脚本吗？

当然可以！

GitHub Actions 有中文文档，根据需求，我们可以去细读文档。

如 [Github Actions 创建操作](https://docs.github.com/cn/actions/creating-actions)，官方有提供创建 javascript 脚本和 docker 脚本的教程。

实际上我们也可以创建其他语言的脚本，例如python。 

#### 示例：javascript创建脚本

创建一个可以打印`hello world`的action [hello-world-javascript-action](https://github.com/blak-kong/hello-world-javascript-action.git)

**1. actions.yml 脚本，定义工作流。**

该脚本可以通过who-to-greet，输入一个变量

```yml
name: 'Hello World' # name 定义工作流的名称
description: 'Greet someone and record the time' # 对该工作流要完成的任务进行简单的描述.
inputs: # 输入类
  who-to-greet:  # 变量 id of output
    description: '测试输入描述'
    required: true   # 必须存在
    default: 'World' # 默认值
outputs: # 输出类
  time: # 变量 id of output
    description: '测试输出描述'
runs: # 运行
  using: 'node12' # 使用 node12 运行
  main: 'index.js' # 运行入口
```


**2. 脚本运行具体操作。**

```javascript
const core = require('@actions/core');
const github = require('@actions/github');

try {
  // `who-to-greet` input defined in action metadata file
  // 获取 action.yml 中 input 类 定义的变量
  const nameToGreet = core.getInput('who-to-greet');
  console.log(`Hello ${nameToGreet}!`);
  const time = (new Date()).toTimeString();

  // 输出 action.yml 中 output 类 定义的变量
  core.setOutput("time", time);


  // Get the JSON webhook payload for the event that triggered the workflow
  // 触发工作流完成钩子
  const payload = JSON.stringify(github.context.payload, undefined, 2)
  console.log(`The event payload: ${payload}`);
} catch (error) {
  core.setFailed(error.message); // 报错
}
```

**3. workflow（持续集成运行一次，就是一个工作流程）**
```yml
on: [push] # push 操作执行自动化

jobs: # 任务
  hello_world_job:
    runs-on: ubuntu-latest
    name: A job to say hello
    steps:
      # 要使用此仓库的私有操作，必须检出仓库（未发布的actions，需要用actions/checkout）
      # 此操作会重新拉取仓库
    - name: Checkout
      uses: actions/checkout@v2
    - name: Hello world action step
      id: hello123
      # uses: actions/hello-world-javascript-action@v1
      uses: ./ # 引入使用自定义的脚本,相对路径为私有操作，非第三方引用
      with:
        who-to-greet: '测试'
    # Use the output from the `hello` step
    - name: 获取输出时间
      run: echo "时间是 ${{ steps.hello123.outputs.time }}"
```


**运行结果**

[![运行结果](https://s1.ax1x.com/2020/07/24/UvsCAf.png)](https://imgchr.com/i/UvsCAf)

## 具体使用

GitHub Actions 有一些自己的术语。

（1）workflow （工作流程）：持续集成一次运行的过程，就是一个 workflow。

（2）job （任务）：一个 workflow 由一个或多个 jobs 构成，含义是一次持续集成的运行，可以完成多个任务。

（3）step（步骤）：每个 job 由多个 step 构成，一步步完成。

（4）action （动作）：每个 step 可以依次执行一个或多个命令（action）。


对于需要加密的环境变量，以及在工作流中使用docker镜像，此类具体用法应该查看官方文档。

配置和管理工作流程 - GitHub Docs  https://docs.github.com/cn/actions/configuring-and-managing-workflows
