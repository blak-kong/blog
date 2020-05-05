---
title: 使用css module替代scopet
date: 2018-04-08 20:26:53
categories: 前端
tags:
  - css
  - css模块化
---

[toc]

# 关于CSS Module

- css modules是一种流行的模块化和组合CSS的系统。 vue-loader提供了与css modules的集成，作为scope CSS的替代方案。

<!-- more -->

## vue引入scopet，有缺陷的样式私有化
最开始的时候，我们提倡并大量使用的是scoped这种技术
在vue组件中，为了使样式私有化（模块化），不对全局造成污染，可以在style标签上添加scoped属性，以表示它的只属于当下的模块，这是一个非常好的举措。
	<style scoped>
	  @media (min-width: 250px) {
    .list-container:hover {
      background: orange;
    }
	}
	</style>

    <!-- more -->

这个可选 scoped 属性会自动添加一个唯一的属性 (比如 data-v-21e5b78) 为组件内 CSS 指定作用域
编译的时候 .list-container:hover 会被编译成类似 .list-container[data-v-21e5b78]:hover

- 但是，它并不能完全避免冲突

如果用户在不同的父子级，定义了一个重复的类名，会影响到所有定义为errShow类名的组件的显示
并且它会造成一种后果，每个样式的权重加重了：
理论上我们要去修改这个样式，需要更高的权重去覆盖这个样式。
这是增加复杂度的其中一个维度。

### CSS modules则做的更好，它不是添加属性，而是直接改变类名

SS Modules既不是官方标准，也不是浏览器的特性，而是在构建步骤中对CSS类名选择器限定作用域的一种方式（通过hash实现类似于命名空间的方法）。

- 类名是动态生成的，唯一的，并准确对应到源文件中的各个类的样式。

实际上，CSS Modules只是CSS模块化的一种方式。为什么我们需要CSS模块化呢？

CSS的规则都是全局的，任何一个组件的样式规则，都对整个页面有效。于是，亟待解决的就是样式冲突（污染）的问题。一般地，为了解决冲突，会把class命名写长一点，降低冲突几率；加上父元素的选择器，来限制范围等……

CSS模块化就是来解决这个问题的，一般地，分为三类

　　1、命名约定类

　　该类CSS模块化方案主要用来规范CSS命名，最常见的是BEM，当然还有OOCSS等，在构建工具出现之前，大多数都是在CSS命名上做文章

　　2、CSS in JS

　　彻底抛弃CSS，用javascript来写CSS规则，常见的有styled-components

　　3、使用JS来管理样式

　　使用JS编译原生的CSS文件，使其具备模块化的能力，最常见的就是CSS Modules

　　随着构建工具的兴起，越来越多的人开始使用后两者方案，书写CSS时，不用再特意地关心样式冲突问题。只需要使用约定的格式编写代码即可

## VUE的CSS Module写法

- 使用时需要进行添加`v-bind`，如样式绑定简写 `:class`

① 在style标签中添加module属性，表示打开CSS-loader的模块模式

	<style module>
	.red {color: red;}
	</style>

② 在模板中使用动态类绑定 `:class` ，并在类名前面加上 `$style.`

	<template>
	  <p :class="$style.red">
	    This should be red
	  </p>
	</template>

③ 如果类名包含中划线，则使用中括号语法

	<h4 :class="$style['header-tit']">类别推荐</h4>

④ 也可以使用数组或对象语法

	<p :class="{ [$style.red]: isRed }">
	      Am I red?
	</p>
	<p :class="[$style.red, $style.bold]">
	      Red and bold
	</p>

⑤ 更复杂的对象语法

	    <ul
	　　　 :class="{
	        [$style.panelBox]:true,
	        [$style.transitionByPanelBox]:needTransition
	      }"
⑥ 更复杂的数组语法

	<li
	      :class="[
	        $style['aside-item'],
	        {[$style['aside-item_active']]:(i === index)}
	      ]"


## 配置

css-loader关于CSS modules的默认配置如下

	{
	  modules: true,
	  importLoaders: 1,
	  localIdentName: '[hash:base64]'
	}

可以使用vue-loader的cssModules选项为css-loader进行自定义的配置

	module: {
	  rules: [
	    {
	      test: '\.vue$',
	      loader: 'vue-loader',
	      options: {
	        cssModules: {
	          localIdentName: '[path][name]---[local]---[hash:base64:5]',
	          camelCase: true
	        }
	      }
	    }
	  ]
	}
