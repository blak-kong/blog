---
title: vue动画
date: 2018-05-01 22:32:07
categories:
  - vue
tags:
  - vue
---

## 单元素/组件的过渡

Vue 提供了 transition 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡
- 条件渲染 (使用 v-if)
- 条件展示 (使用 v-show)
- 动态组件
- 组件根节点

vue的动画也是使用css3实现的。
只是写法有些不一样，我们需要将代码嵌套在<transition>中

<!--more-->

## 过渡的类名

而与css动画最大的区别，我们需要使用vue自带的模板语法
通过Vue自带的类名，我们可以定义css动画的触发时机

在进入/离开的过渡中，会有 6 个 class 切换。

- v-enter：定义进入过渡的开始状态。
  - 在元素被插入之前生效，在元素被插入之后的下一帧移除。

- v-enter-active：定义进入过渡生效时的状态。
  - 在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。

- v-enter-to: 2.1.8版及以上 定义进入过渡的结束状态。
  - 在元素被插入之后下一帧生效 (与此同时 v-enter 被移除)，在过渡/动画完成之后移除。

- v-leave: 定义离开过渡的开始状态。
  - 在离开过渡被触发时立刻生效，下一帧被移除。

- v-leave-active：定义离开过渡生效时的状态。
  - 在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。

- v-leave-to: 2.1.8版及以上 定义离开过渡的结束状态。
  - 在离开过渡被触发之后下一帧生效 (与此同时 v-leave 被删除)，在过渡/动画完成之后移除。

```

.fade-enter-active,.fade-leave-active{
			transition:all 3s ease;
		}
		.fade-enter-active{
			opacity:1;// 动画开始状态
			width:300px;
			height:300px;
		}
		.fade-leave-active{
			opacity:0;
			width:50px;
			height:50px;
		}
		/* .fade-enter需要放在.fade-enter-active的后面 */
		.fade-enter{
			opacity:0; /*初始状态（css重叠样式表，属性覆盖，必须放后面）*/
			width: 100px;
			height: 100px;
		}
```

## javaScript 钩子

可以在属性中声明 JavaScript 钩子

```
<transition name="fade" 
			@before-enter="beforeEnter"// 动画进入之前
			@enter="enter"// 动画进入
			@after-enter="afterEnter"// 动画进入之后

			@before-leave="beforeLeave"// 动画即将离开之前
			@leave="leave"// 动画离开
			@after-leave="afterLeave"// 动画离开之后
		>
			<p v-show="flag">网博</p>
		</transition>
```

钩子函数的触发时机
```
methods:{
				beforeEnter(el){
					// alert('动画进入之前');
				},
				enter(){
					// alert('动画进入');
				},
				afterEnter(el){
					// alert('动画进入之后');
					el.style.background='blue';
				},

				beforeLeave(){
					// alert('动画即将之前');
				},
				leave(){
					// alert('动画离开');
				},
				afterLeave(el){
					// alert('动画离开之后');
					el.style.background='red';
				}
			}
```

这些钩子函数可以结合 CSS transitions/animations 使用，也可以单独使用。

>当只用 JavaScript 过渡的时候， 在 enter 和 leave 中，回调函数 done 是必须的 。
>否则，它们会被同步调用，过渡会立即完成。

>推荐对于仅使用 JavaScript 过渡的元素添加 v-bind:css="false"，Vue 会跳过 CSS 的检测。
>这也可以避免过渡过程中 CSS 的影响。

# 在vue中使用animations.css动画库

只需要引入animations.css，然后写法如下，在标签中直接定义开始动画和离开动画即可

```
<transition enter-active-class="animated fadeInLeft" leave-active-class="animated fadeOutRight">
			<p v-show="flag">网博</p>
		</transition>
```

## 多元素动画

对于原生标签可以使用 v-if/v-else 。

当有相同标签名的元素切换时，需要通过 key 特性设置唯一的值来标记以让 Vue 区分它们，否则 Vue 为了效率只会替换相同标签内部的内容。
即使在技术上没有必要，给在 `<transition>` 组件中的多个元素设置 key 也是一个更好的实践。
```
<transition enter-active-class="animated bounceInLeft" leave-active-class="animated bounceOutRight">
			<p v-show="flag" :key="1">itany</p>
			<p v-show="flag" :key="2">网博</p>
</transition>
```

我们也可以通过给同一个元素的 key 特性设置不同的状态
来代替 v-if 和 v-else

```
<transition>
  <button v-bind:key="isEditing">
    {{ isEditing ? 'Save' : 'Edit' }}
  </button>
</transition>
```

或者使用多个if

## 列表过渡

同时渲染整个列表`<transition-group>`

- 不同于 <transition>，它会以一个真实元素呈现：默认为一个 <span>。你也可以通过 tag 特性更换为其他元素。
- 内部元素 总是需要 提供唯一的 key 属性值

```
<div id="itany">
		<input type="text" v-model="name">
		
		<transition-group enter-active-class="animated bounceInLeft" leave-active-class="animated bounceOutRight">
			<p v-for="(v,k) in arr2" :key="k" v-show="flag">
				{{v}}
			</p>
		</transition-group>
	</div>

	<script>
		var vm=new Vue({
			el:'#itany',
			data:{
				flag:true,
				arr:['tom','jack','mike','alice','alex','mark'],
				name:''
			},
			computed:{
				arr2:function(){
					var temp=[];
					this.arr.forEach(val => {
						// Array.prototype.includes(): 判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回false。
						if(val.includes(this.name)){ // 判断是否存在传入字符，存在则push元素
							temp.push(val);
						}
					});
					return temp;
				}
			}
		});
	</script>
```

检测元素数组元素是否存在：
Array.prototype.includes(): 判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回false。