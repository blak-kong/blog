---
title: 组件component
date: 2018-05-03 11:55:47
categories:
  - vue
tags:
  - vue组件 
---
[toc]

## 什么是组件？

组件（Component）是 Vue.js 最强大的功能之一。
组件可以扩展 HTML 元素，封装可重用的代码
组件是自定义元素（对象）

<!--more-->

## 定义组件的方式

#### 方式1：先创建组件构造器，然后由组件构造器创建组件（全局）
```
1.使用Vue.extend()创建一个组件构造器
		var MyComponent=Vue.extend({
			template:'<h3>Hello World</h3>'
		});
2.使用Vue.component(标签名,组件构造器)，根据组件构造器来创建组件
		Vue.component('hello',MyComponent);
```

#### 方式2：直接创建组件（全局）

直接把组件构造器写在{}里定义

```
Vue.component('my-world',{
			template:'<h1>你好，世界</h1>'
		});
```

#### 方式3：局部组件（常用）

```
// 挂载到单页面里，只在当前页有效
components:{
    'my-world',{
			template:'<h1>你好，世界</h1>'
		}
}
```

## 组件中添加数据

在组件中存储数据时，必须以函数形式，函数返回一个对象
```
components:{ //局部组件
				'my-world':{
					template:'<h3>{{age}}</h3>',
					data(){
						return {
							age:25
						}
					}
				}
			}
```

## 引用模板

组件化开发中，引用模板为直接创建新文件，作为组件。
组件名为文件名。

```
<template id="wbs">
	<!-- <template>必须有且只有一个根元素 -->
	<div></div>
</template>

components:{
	'my-hello':{
		name:'wbs17022',  //指定组件的名称，默认为标签名，可以不设置
		template:'#wbs',
		data(){
			return {
				msg:'欢迎来到南京网博',
				arr:['tom','jack','mike']
			}
		}
	}
				
}
```

## 动态加载组件

`<component :is="挂载点">`
    多个组件使用同一个挂载点，然后动态的在它们之间切换    

```
<button @click="flag='my-hello'">显示hello组件</button>
<button @click="flag='my-world'">显示world组件</button>

<component :is="flag"></component>

data:{
		flag:'my-hello'
	}
```

## `<keep-alive>`缓存组件

标签的组件实例能够被在它们第一次被创建的时候缓存下来。
