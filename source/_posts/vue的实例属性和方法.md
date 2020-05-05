---
title: vue的实例属性和方法
date: 2018-05-01 22:31:14
categories: 前端 vue
tags:
  - vue
  - vue实例属性 计算属性 
---

[TOC]

# vue实例的属性和方法
仅记录部分
大部分时候是随学随用，但是学到了就来记录。

>实例属性和方法 == 组件实例和方法 != 全局属性和方法

- 另外，在组件实例的`methods方法`中不能使用全局Vue方法

<!-- more -->

## 属性

#### vm.属性名 获取data中的属性
```
//console.log(vm.msg);
```

#### vm.$el 获取vue实例关联的元素
```
// console.log(vm.$el); //DOM对象
// vm.$el.style.color='red';
```

#### vm.$data 获取数据对象data
```
// console.log(vm.$data);
// console.log(vm.$data.msg);
```

#### vm.$options 获取自定义属性
```
// console.log(vm.$options.name);
// console.log(vm.$options.age);
// vm.$options.show();
```

#### vm.$refs 获取所有添加ref属性的元素
```
// console.log(vm.$refs);
// console.log(vm.$refs.hello); //DOM对象
// vm.$refs.hello.style.color='blue';
```

## 方法


#### vm.$mount()  手动挂载vue实例

		// vm.$mount('#itany');
		var vm=new Vue({
			data:{
				msg:'欢迎来到南京网博',
				name:'tom'
			}
		}).$mount('#itany');

#### vm.$destroy() 销毁实例
这个列出来只是了解一下，官方也建议使用`v-if`或`v-for`以数据驱动的方式控制子组件和生命周期

		// vm.$destroy();

#### vm.$nextTick(callback) 在DOM更新完成后再执行回调函数，一般在修改数据之后使用该方法，以便获取更新后的DOM

		//修改数据
		vm.name='汤姆';
		//DOM还没更新完，Vue实现响应式并不是数据发生改变之后DOM立即变化，需要按一定的策略进行DOM更新，需要时间！！
		// console.log(vm.$refs.title.textContent);
		vm.$nextTick(function(){
			//DOM更新完成，更新完成后再执行此代码
			console.log(vm.$refs.title.textContent);
		});

#### vm.$set(target,key,value) 添加对象的属性和值

[关于dom](https://github.com/stone0090/javascript-lessons/tree/master/2.2-DOM)

// vm.$set(this.food, 'count', 1);
// 注意：在`methods方法`中不能使用`Vue.set`
// 如果在实例创建之后添加新的属性到实例上，它不会触发视图更新
// Tip:Vue.set()在methods中可以写成this.$set()

#### vm.$delete(target,key) 删除对象的属性

// vm.$delete(this.food, 'count');
// 注意：在`methods方法`中不能使用`Vue.delete`

#### vm.$watch( expOrFn, callback, [options] ) 观察者模式

参数：监听对象，回调方法，选项（deep/immediate）

观察 Vue 实例变化的一个表达式或计算属性函数。回调函数得到的参数为新值和旧值。
表达式只接受监督的键路径。对于更复杂的表达式，用一个函数取代。

实例使用方法
```
//方式1：使用vue实例提供的$watch()方法
		vm.$watch('name',function(newValue,oldValue){
			console.log('name被修改啦，原值：'+oldValue+'，新值：'+newValue);
		});
```
vue提供的选项方法
```
watch:{ //方式2：使用vue实例提供的watch选项
	age:(newValue,oldValue) => {
		console.log('age被修改啦，原值：'+oldValue+'，新值：'+newValue);
	},
    // 对对象监视，需要使用深度监视
	user:{
		handler:(newValue,oldValue) => {
		console.log('user被修改啦，原值：'+oldValue.name+'，新值：'+newValue.name);
	},
	deep:true //深度监视，当对象中的属性发生变化时也会监视
	}
}
```

#### vm.$emit( event, […args] ) 触发当前实例上的事件。附加参数都会传给监听器回调。
