---
title: 【HTML】排版
date: 2020-05-05 21:10:39
tags:
  - html
  - 网页排版
  - 他人博客
---

[toc]

## 一、 网页排版

排版标签：

- `<h1>`

- `<p>`

- `<hr />` 水平分割线标签

- `<br />` 换行标签

- `<div>`

- `<span>`

- `<pre>` 预定义标签：原样输出内容

原文地址： [HTML 标签：排版标签](https://github.com/qianguyihao/Web/blob/master/01-HTML/04-HTML%E6%A0%87%E7%AD%BE%EF%BC%9A%E6%8E%92%E7%89%88%E6%A0%87%E7%AD%BE.md)

<!-- more -->

## 二、字体与锚点链接

**个人简要笔记：**

### 2-1.字体

```
// b s u i 都是已经废弃的，现在用左边更语义化的标签
strong == b // strong 粗体标签 语义: 定义重要性强调的文字
del == s // del 中划线/删除线 语义(deleted): 定义被删除的文字
ins == u // ins 下划线 语义(inseted): 定义插入的文字
em == i // em 斜体 语义(emphasized): 定义强调的文字
```

### 2-2.锚点链接

锚点链接即是通过给元素设置 id（设置标记），实现浏览器可以在链接访问网页的时候，在根据链接地址后面的`#id`参数，来自动定位到网页文档的对应位置。

> 锚链接的作用是在本页面或者其他页面的的不同位置进行跳转。

比如说，在网页底部有一个向上箭头，点击箭头后回到顶部，这个就可以利用锚链接。

不过锚链接没有动画，是立即跳转到对应位置。所以实际使用中，往往需要依赖别人封装好的插件。

例如`navScroll.ja`

参考原文地址： [HTML 标签：字体标签和超链接](https://github.com/qianguyihao/Web/blob/master/01-HTML/05-HTML%E6%A0%87%E7%AD%BE%EF%BC%9A%E5%AD%97%E4%BD%93%E6%A0%87%E7%AD%BE%E5%92%8C%E8%B6%85%E9%93%BE%E6%8E%A5.md)

## 三、图片标签

主要注意 align 属性

图片的 align 属性：图片和周围文字的相对位置。

属性取值可以是：bottom（默认）、center、top、left、right。

如果想实现图文混排的效果，请使用 align 属性，取值为 left 或 right。

我们来分别看一下这 align 属性的这几个属性值的区别。

1、align=""，图片和文字低端对齐。即默认情况下的显示效果

2、align="center"：图片和文字水平方向上居中对齐。 3、align="top"：图片与文字顶端对齐。

4、align="left"：图片在文字的左边。

5、align="right"：图片在文字的右边。

原文地址： [HTML 标签：图片标签](https://github.com/qianguyihao/Web/blob/master/01-HTML/06-HTML%E6%A0%87%E7%AD%BE%EF%BC%9A%E5%9B%BE%E7%89%87%E6%A0%87%E7%AD%BE.md)
