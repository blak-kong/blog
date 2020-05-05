---
title: sass使用之mixin
date: 2018-04-08 20:21:14
categories: 前端
tags:
  - CSS
  - SASS
---
目前仅用于最简单的实战，没有深入学习，以下介绍SASS常用方法

## 定义mixin

通过 `@mixin  名称` 的方式可以定义一个Mixins模块：在模块内你可以添加任何你想重复使用的样式。

<!-- more -->

示例（以下全文使用一个例子）：

	@mixin flex($direction:column,$inline:block) {
	    display: if($inline==block,flex,inline-flex);
	    flex-direction: $direction;
	    flex-wrap: wrap;
	}

由于历史原因，连字符和下划线被认为是相同的，也就是说 @mixin button-large { } 和 @mixin button_large { } 是一样的。

Mixins能够包含任何在 CSS 和 Sass 中有效的内容。

<!-- more -->

## 调用Mixin

你可以通过 `@include` 来调用具有相同名称的mixin模块。

	@mixin btn {
		display:inline-block;
	}
	.btn {
	    @include btn;
	    //等于display:inline-block;
	}

-  `@include` 也可以在mixin模块的定义中使用

## 参数的使用

使用`$`，Mixins可以接收和使用参数，这有点像less，但作用域不同

	@mixin btn($size:14px,$color:#fff,$bgcolor:#f04752,$padding:5px,$radius:5px) {
	    padding: $padding;
	    background-color: $bgcolor;
	    border-radius: $radius;
	    border: 1px solid $bgcolor;
	    font-size: $size;
	    color: $color;
	    text-align: center;
	    line-height: 1;
	    display: inline-block;
	}

## 关键字参数

为了帮助你的代码更加容易理解，你可以在传递值给mixin时将参数名称和参数值一并传递过去。

	button-green { @include button($background: green, $color: #fff); }

关键字参数会额外增加一些代码，但是这会使得你的@include更加容易理解。
上面的代码明确指出了green和#fff分别是什么。

## 数量可变的参数

Mixins可以接收未知数量的参数。
通过在变量名后增加三个点（...）来使mixin模块接收数量可变的参数。
当你使用@include传递参数的时候，使用逗号将参数分开。

	@mixin box-shadows($shadow...) {
	          box-shadow: $shadow;
	}

	 .container {
	         @include box-shadows(0px 1px 2px #333, 2px 3px 4px #ccc);  
	 }

编译为

	.container {
	         box-shadow: 0px 1px 2px #333,
	                     2px 3px 4px #ccc;
	         }
