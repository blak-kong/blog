---
title: CSSOM视图模式
date: 2018-05-02 13:26:18
categories:
  - JS学习笔记
tags:
  - 浏览器视图属性
  - 页面内元素移动和滚动
---

# CSSOM视图模块(CSSOM View Module)

它定义了一些 API，Web 开发人员使用这些 API 可以进行检查，也可以以编程方式更改文档及其内容的视觉属性

包括布局框定位、视区宽度和元素滚动。

以下就是一些API属性的相关内容，包括兼容性，使用，测试等。

<!--more-->

## window视图属性（几乎没用）

这些属性可以hold住整个浏览器窗体大小。微软则将这些API称为“Screenview 接口”。

>语法：window.xxxxx

它们一般没什么用，仅作了解：
- innerWidth 属性和 innerHeight 属性
    - 表示获取window窗体的内部宽高，不包括用户界面元素，比如窗框。
- pageXOffset 属性和 pageYOffset 属性
    - 表示整个浏览器窗体的大小，包括任务栏等。
- screenX 属性和 screenY 属性
    - 表示整个页面滚动的像素值（水平方向的和垂直方向的）
- outerWidth 属性和 outerHeight 属性
    - 浏览器窗口在显示器中的位置，screenX表示水平位置，screenY表示垂直位置。

## Screen视图属性（几乎没用）

>语法：screen.xxxxx

- availWidth和availHeight
    - 显示器可用宽高，不包括任务栏之类的东东。
- width和height
    - 表示显示器屏幕的宽高。

## 元素视图属性 (重点)

使用方法，自己获取页面元素，然后对元素应用方法。

- clientLeft和clientTop（事件内容的位置）
    - 表示内容区域的左上角相对于整个元素左上角的位置（包括边框）。


- clientWidth和clientHeight（事件内容的大小）
    - 表示内容区域的高度和宽度，包括padding大小，但是不包括边框和滚动条。

- offsetLeft和offsetTop（检测位置）
    - 表示相对于最近的祖先定位元素（CSS position 属性被设置为 relative、absolute 或 fixed 的元素）的左右偏移值。


- offsetWidth和offsetHeight（检测大小）
    - 整个元素的尺寸（包括边框）。

- scrollLeft和scrollTop（用于滚动）
    - 表示元素滚动的像素大小。可读可写。

- scrollWidth和scrollHeight（整个内容的宽高-包括滚动）
    - 表示整个内容区域的宽高，包括隐藏的部分。如果元素没有隐藏的部分，则相关的值应该等用于clientWidth和clientHeight。当你向下滚动滚动条的时候，scrollHeight应该等用于scrollTop + clientHeight。


## 鼠标位置 （重要）

>语法：event.clientX

- clientX,clientY
    - 相对于window，为鼠标相对于window的偏移。

>用于点击移动

- offsetX, offsetY
    - 表示鼠标相对于当前被点击元素padding box的左上偏移值，各个浏览器的兼容性五花八门.

>获取鼠标在元素内的位置

- pageX, pageY（兼容性不好）
    - 为鼠标相对于document的坐标。

- screenX, screenY
    - 鼠标相对于显示器屏幕的偏移坐标。

#### clientXY+offsetXY组合用法

>clienX-offsetX = 元素不超出页面

因为window宽度就是浏览器宽度。
