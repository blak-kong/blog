---
title: vue生命周期和计算属性
date: 2018-05-01 22:30:52
categories: 前端 vue
tags:
  - vue
  - 生命周期 计算属性 
---

# 生命周期

## vue有八个生命周期，但只有两个是常用的

新手阶段只需要掌握created()和mounted()即可

<!-- more -->

#### created(): 实例已经创建完成，并且已经进行数据观测和事件配置

此时可用于添加数据、配置事件

#### mounted(): 模板编译之后，已经挂载元素，此时才会渲染页面，才能看到页面上数据的展示

此时可用于操作html和dom元素

<!-- more -->

```
beforeCreate(){
	alert('组件实例刚刚创建，还未进行数据观测和事件配置');
},
created(){  //常用！！！
	alert('实例已经创建完成，并且已经进行数据观测和事件配置');
},
beforeMount(){
	alert('模板编译之前，还没挂载');
},
mounted(){ //常用！！！
	alert('模板编译之后，已经挂载，此时才会渲染页面，才能看到页面上数据的展示');
},
beforeUpdate(){
	alert('组件更新之前');
},
updated(){
	alert('组件更新之后');
},
beforeDestroy(){
	alert('组件销毁之前');
},
destroyed(){
	alert('组件销毁之后');
}
```

# 计算属性 `computed`

`computed`：对于两个以上操作的复杂逻辑，应当使用计算属性（不建议仅有一个操作的简单逻辑）。


一个简单实例-反转字符串
html代码
```
<p>{{ reversedMessage }}</p> 这里会输出olleh 
```
js代码
```
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello' // 绑定的字符串
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
      // split('')把每个字符分割，形成数组
      // reverse()反转数组
      // join('')拼接数组元素，返回字符串
    }
  }
})
```

通过上面的实例，我们看到在计算属性中，当我们定义方法时，是使用了计算属性的一个getter。
但是getter和setter往往是一对的，分别是只读只写。
所以这个实例，是不是漏了什么呢？

没错，它还不足以讲解计算属性，
这里只是说明了它可以通过调用使用。

另外，我们可以像绑定普通属性一样在模板中绑定计算属性。
```
console.log(vm.reversedMessage) // => 'olleH'
vm.message = 'Goodbye'
console.log(vm.reversedMessage) // => 'eybdooG'
```

## 计算属性缓存 vs 方法

我们可以将同一函数定义为一个方法而不是一个计算属性。
两种方式的最终结果确实是完全相同的。然而，不同的是计算属性是基于它们的依赖进行缓存的。

#### `computed计算属性` 必须有计算 

#### `methods方法` 一般用于处理事件、改变状态。

#### 计算属性只有在它的相关依赖发生改变时才会重新求值（设计初衷是为了缓存）。

这就意味着只要`数据`还没有发生改变，即便多次访问`方法`，计算属性也会立即返回之前的计算结果，而不必再次执行函数。
这也意味着计算属性将不再更新。

## 计算属性 vs 侦听属性

Vue 提供了一种更通用的方式来观察和响应 Vue 实例上的数据变动：侦听属性。
当你有一些数据需要随着其它数据变动而变动时，你很容易滥用 watch

然而，通常更好的做法是使用计算属性而不是命令式的 watch 回调。

## 计算属性的 setter

计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter。
通过给set传值，可以间接改变该计算属性的值。
#### （绝对不能直接给计算属性赋值）

```
data:{ //普通属性
	num1:8
},
computed:{ //计算属性
	num2:{
		get:function(){
			console.log('num2：'+new Date());
			return this.num1-1;
		},
		set:function(val){ // val=111
			// console.log('修改num2值');
			// this.num2=val; 不能直接赋值num2，那不是计算,会死循环溢出
			this.num1=val;// 赋值修改num1
		}
	}
},
methods:{
	change(){
		// this.msg='i love you';
		this.num1=666;// 赋值修改num1
	},
	getNum2(){
		console.log(new Date());
		return this.num1-1;
	},
	change2(){
		this.num2=111; //传入set
	}
}
```

## 侦听器

一旦发生改变，就会运行，watch中可以自定义函数
```
data: {
    obj: '',
    answer: '请输入!'
  },
watch: {
    // 如果 `obj` 发生改变，这个函数就会运行
    obj: function (新的值, 旧的值) {
      this.answer = '每次监听到getAnswer()发生变化，就把这句话赋值给answer，反正还是会被getAnswer()覆盖'
      this.getAnswer()// 还在methods方法里自定义要监听调用的函数。
    }
  },
```