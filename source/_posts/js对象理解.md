---
title: js对象理解
date: 2018-04-29 14:52:23
categories:
  - JS学习笔记
tags:
  - 对象 js
---
[toc]

# 创建对象

对象最常见的用法是创建（create）、设置（set）、查找（query）、删除（delete）、检测（test）和枚举（enumerate）它的属性。

 JavaScript 的「三类对象」和「两类属性」进行区分：

内置对象（native object），是由 JavaScript 规范定义的对象或类。例如，数组、函数、日期和正则表达式都是内置对象。
宿主对象（host object），是由 JavaScript 解释器所嵌入的宿主环境（比如 Web 浏览器）定义的。客户端 JavaScript 中表示网页结构的 HTMLElement 对象均是宿主对象。
自定义对象（user-defined object），是由运行中的 JavaScript 代码创建的对象。

自有属性（own property），是直接在对象中定义的属性。
继承属性（inherited property），是在对象的原型对象中定义的属性。
<!-- more -->

## 使用对象字面量创建对象（推荐）

```
// 推荐写法
var person = {
    name : "stone",
    age : 28
};

// 也可以写成
var person = {};
person.name = "stone";
person.age = 28;
```

## 使用 `new` 关键字创建对象

`new` 关键字`创建并初始化`一个新对象。关键字 new 后跟随一个`函数调用`。
这里的函数称做构造函数（constructor），`构造函数用以初始化一个新创建的对象`。
JavaScript 语言核心中的原始类型都包含内置构造函数。例如：
```
var person = new Object();
person.name = "stone";
person.age = 28;
```
- 只要在 `new` 后面跟随一个已创建的对象，就可以继承它的原型属性。 

## 使用 `Object.create()` 函数创建对象

`Object.create()` 创建一个具有指定原型且可选择性地包含指定属性的对象。

>Object.create(prototype, descriptors)

参数
prototype
必需。  要用作原型的对象。  可以为 null。  
descriptors
可选。  包含一个或多个属性描述符的 JavaScript 对象。  
“数据属性”是可获取且可设置值的属性。
数据属性描述符包含 value(值) 特性，以及 writable(可写)、enumerable（可枚举） 和 configurable（可配置） 特性。
如果未指定最后三个特性，则它们默认为 false。
只要检索或设置该值，“访问器属性”就会调用用户提供的函数。
访问器属性描述符包含 set 特性和/或 get 特性。

下面的示例创建使用 null 原型的对象并添加两个可枚举的属性。

```
var newObj = Object.create(null, {
            size: {
                value: "large",
                enumerable: true
            },
            shape: {
                value: "round",
                enumerable: true
            }
        });

console.log(newObj.size + "<br/>");
console.log(newObj.shape + "<br/>");
console.log(Object.getPrototypeOf(newObj));

// Object.getPrototypeOf 函数可获取原始对象的原型。

// 输出(Output):
// large
// round
// null
```

## 原型（prototype）

所有通过对象字面量创建的对象都具有同一个原型对象，并可以通过 JavaScript 代码 Object.prototype 获得对原型对象的引用。
Object.prototype 是所有对象的原型对象，也是原型链终点，但是它默认是null的。
Object.prototype.__proto__为null （普通的对象）
Object.prototype 的存在价值就是象征Object类型本身,是根对象

通过关键字 new 和构造函数调用创建的对象的原型就是构造函数的 prototype 属性的值。

因此，同使用 {} 创建对象一样，通过 new Object() 创建的对象也继承自 Object.prototype。
同样，通过 new Array() 创建的对象的原型就是 Array.prototype，通过 new Date() 创建的对象的原型就是 Date.prototype。

没有原型的对象为数不多，Object.prototype 就是其中之一。它不继承任何属性。
其他原型对象都是普通对象，普通对象都具有原型。

所有的内置构造函数（以及大部分自定义的构造函数）都具有一个继承自 Object.prototype 的原型。
例如，Date.prototype 的属性继承自 Object.prototype，因此由 new Date() 创建的 Date 对象的属性同时继承自 Date.prototype 和 Object.prototype。

这一系列链接的原型对象就是所谓的「原型链（prototype chain）」。

#### 注意：只有函数才有显式原型属性（prototype），对象只有隐式原型（__proto__）属性

函数的prototype属性:在定义函数时自动添加的,默认值是一个空的Object对象
对象的__proto__属性:创建对象是自动创建、添加的,默认值为其对应构造函数的prototype属性值

## 属性的查询和设置
```
//设置
person.name = "sophie"; // 赋值
//查询
console.log(person.name);   // "sophie"
```

## 属性的访问错误

查询一个不存在的属性并不会报错，如果不存在，则返回undefined

## 删除属性
JavaScript 对象可以看做属性的集合，我们经常会检测集合中成员的所属关系（判断某个属性是否存在于某个对象中）。。可以通过 in 运算符、hasOwnPreperty() 和 propertyIsEnumerable() 来完成这个工作，甚至仅通过属性查询也可以做到这一点。

in 运算符的左侧是属性名（字符串），右侧是对象。
如果对象的自有属性或继承属性中包含这个属性则返回 true。

```
var o = { x: 1 }
console.log("x" in o);          // true，x是o的属性
console.log("y" in o);          // false，y不是o的属性
console.log("toString" in o);   // true，toString是继承属性
```

除了使用 in 运算符之外，另一种更简便的方法是使用 !== 判断一个属性是否是 undefined

 `hasOwnProperty()` 方法用来检测给定的名字是否是对象的`自有属性`。
对于继承属性它将返回 false。

`propertyIsEnumerable()` 检测对象有`自有属性`且`可枚举性`为 true 时它才返回 true。

## 枚举属性

除了检测对象的属性是否存在，我们还会经常遍历对象的属性。通常使用 for-in 循环遍历，ECMAScript 5 提供了两个更好用的替代方案。

for-in 循环可以在循环体中遍历对象中所有可枚举的属性（包括自有属性和继承的属性），把属性名称赋值给循环变量。
对象继承的内置方法不可枚举的，但在代码中给对象添加的属性都是可枚举的。例如：
```
var o = {x:1, y:2, z:3};            // 三个可枚举的自有属性
o.propertyIsEnumerable("toString"); // false，内置方法不可枚举
for (p in o) {          // 遍历属性
    console.log(p);     // 输出x、y和z，不会输出toString
}
```

有许多实用工具库给 Object.prototype 添加了新的方法或属性，这些方法和属性可以被所有对象继承并使用。

为了过滤继承的属性，我们可以对 for-in 进行改造
只许加入两个判断
hasOwnProperty()
这个方法可以用来检测一个对象是否含有特定的自身属性；和 in 运算符不同，该方法会忽略掉那些从原型链上继承到的属性。
typeof 判断对象类型
```
for(p in o) {
   if (!o.hasOwnProperty(p)) continue;          // 跳过继承的属性
   if (typeof o[p] === "function") continue;    // 跳过函数方法
}
```

除了 for-in 循环之外，ECMAScript 5 定义了两个用以枚举属性名称的函数。

第一个是 Object.keys()，它返回一个数组，这个数组由对象中可枚举的自有属性的名称组成。

第二个是 Object.getOwnPropertyNames()，它和 Ojbect.keys() 类似，只是它返回对象的所有自有属性的名称，而不仅仅是可枚举的属性。

## 属性的 getter 和 setter

由 getter 和 setter 定义的属性称做「存取器属性（accessor property）」
它不同于「数据属性（data property）」，数据属性只有一个简单的值。
并且和数据属性不同，存取器属性不具有可写性（writable attribute）。

```
var o = {
    // 普通的数据属性
    data_prop: value,

    // 存取器属性都是成对定义的函数
    get accessor_prop() { /*这里是函数体 */ },
    set accessor_prop(value) { /* 这里是函数体*/ }
};
```

存取器属性定义为一个或两个和属性同名的函数，这个函数定义没有使用 function 关键字，而是使用 get 或 set。
注意，这里没有使用冒号将属性名和函数体分隔开，但在函数体的结束和下一个方法或数据属性之间有逗号分隔。

## 序列化对象（JSON）
对象序列化（serialization）是指将对象的状态转换为字符串，也可将字符串还原为对象。
JSON.stringify() 用于将 JavaScript 值转换为 JSON 字符串
JSON.parse() 用于将一个 JSON 字符串转换为对象。

## 关卡

[枚举题](https://github.com/stone0090/javascript-lessons/blob/master/1.7-ObjectObjects/Answer.md)

# 总结

创建对象的三种方式，在默认情况下，都一样。
但是在涉及原型的情况下 new 和 create() ，就产生了区别。至于字面量方法，则是无法继承原型。

`new` 关键字`创建并初始化`一个新对象。
关键字 `new` 后跟随一个`函数调用`，新对象会继承它的原型属性。

工厂模式、构造函数模式、原型模式等……面向对象常用语法

`Object.create()` 创建一个具有`指定原型`且`可选择性地包含指定属性`的对象。

new的高级应用？
