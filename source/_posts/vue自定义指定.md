---
title: vue自定义指定
date: 2018-05-01 22:31:39
categories: vue
tags:
  - vue 
---

[TOC]

# 自定义属性

/**
* 自定义全局指令
* 注：使用指令时必须在指名名称前加前缀v-，即v-指令名称
*/

<!--more-->
```
		Vue.directive('hello',{
			bind(){ //常用！！
				alert('指令第一次绑定到元素上时调用，只调用一次，可执行初始化操作');
			},
			inserted(){
				alert('被绑定元素插入到DOM中时调用');
			},
			update(){
				alert('被绑定元素所在模板更新时调用');
			},
			componentUpdated(){
				alert('被绑定元素所在模板完成一次更新周期时调用');
			},
			unbind(){
				alert('指令与元素解绑时调用，只调用一次');
			}
		});
```



#### 全局注册自定义指令

```
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时调用
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})
```

#### 局部注册

```
directives: {
// 自定义指令名
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}
```

#### 在模板中使用
```
<input v-focus>
```

## 钩子函数参数

- 常用参数为`el`和`binding`

钩子函数        参数1  参数2   参数3
bind: function (el, binding, vnode)


指令钩子函数会被传入以下参数：
- el：指令所绑定的元素，可以用来直接操作 DOM 。可以为它绑定事件、也可以直接修改dom属性。
- binding：一个对象。包含以下属性：
    - name：指令名，不包括 v- 前缀。
    - value：指令的绑定值，例如：v-my-directive="1 + 1" 中，绑定值为 2。
    - oldValue：指令绑定的前一个值，仅在 update 和 componentUpdated 钩子中可用。无论值是否改变都可用。
    - expression：字符串形式的指令表达式。例如 v-my-directive="1 + 1" 中，表达式为 "1 + 1"。
    - arg：传给指令的参数，可选。例如 v-my-directive:foo 中，参数为 "foo"。
    - modifiers：一个包含修饰符的对象。例如：v-my-directive.foo.bar 中，修饰符对象为 { foo: true, bar: true }。
- vnode：Vue 编译生成的虚拟节点。
- oldVnode：上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。

```
Vue.directive('demo', {// demo为指令名
  bind: function (el, binding, vnode) {
    
  }
})
```