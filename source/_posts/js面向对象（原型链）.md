---
title: js面向对象（原型链）
date: 2018-05-01 02:38:44
categories:
  - JS学习笔记
tags:
  - 面向对象 原型链 js
---
[toc]

# 原型及原型链

原型链是一种机制，指的是 JavaScript 每个对象都有一个内置的 __proto__ 属性指向创建它的构造函数的 prototype（原型）属性。
原型链的作用是为了实现对象的继承，要理解原型链，需要先从函数对象、constructor、new、prototype、__proto__ 这五个概念入手。

<!-- more -->

# 函数对象

前面讲过，在 JavaScript 里，函数即对象，程序可以随意操控它们。
比如，可以把函数赋值给变量，或者作为参数传递给其他函数，也可以给它们设置属性，甚至调用它们的方法。
下面示例代码对「普通对象」和「函数对象」进行了区分。

普通对象
```
var o1 = {};
var o2 = new Object();
```

函数对象
```
function f1(){};
var f2 = function(){};
var f3 = new Function('str','console.log(str)');
```

简单的说，凡是使用 function 关键字或 Function 构造函数创建的对象都是函数对象。
而且，只有函数对象才拥有 `prototype` （原型）属性。

## `constructor` 构造函数

函数还有一种用法，就是把它作为构造函数使用。
像 Object 和 Array 这样的原生构造函数，在运行时会自动出现在执行环境中。
此外，也可以创建自定义的构造函数，从而自定义对象类型的属性和方法。

构造原型
```
<script>
// 构造函数首字母应大写，便于区分普通函数
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function(){
        console.log(this.name);
    };
}

var person1 = new Person("Stone", 28, "Software Engineer");
var person2 = new Person("Sophie", 29, "English Teacher");
<script/>
```

## `new` 操作符

要创建 Person 的新实例，必须使用 new 操作符。以这种方式调用构造函数实际上会经历以下4个步骤：

1. 创建一个新对象；
2. 将构造函数的作用域赋给新对象（因此 this 就指向了这个新对象）；
3. 执行构造函数中的代码（为这个新对象添加属性）；
4. 返回新对象。

构造函数与其他函数的唯一区别，就在于调用它们的方式不同。

>归根结底，使用 `new` 关键字，可以在后面跟随一个`函数调用`。

只要通过 new 操作符来调用，那它就可以作为构造函数；
而任何函数，如果不通过 new 操作符来调用，那它跟普通函数也不会有什么两样。

## 构造函数的问题
构造函数模式虽然好用，但也并非没有缺点。
使用构造函数的主要问题，就是每个方法都要在每个实例上重新创建一遍。

>函数名是指针，使用`构造函数调用`创建新对象，解决了内存覆盖问题：
>但所有对象都是深拷贝，并没有变方便。没有必要创建两个完成同样任务的 Function 实例

## `prototype` 原型

我们创建的每个函数都有一个 prototype（原型）属性。
使用原型的好处是可以让所有对象实例共享它所包含的属性和方法。
换句话说，不必在构造函数中定义对象实例的信息，而是可以将这些信息直接添加到原型中，如下面的例子所示。

```
function Person(){}

Person.prototype.name = "Stone";
Person.prototype.age = 28;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
    console.log(this.name);
};

var person1 = new Person();
person1.sayName();   // "Stone"

var person2 = new Person();
person2.sayName();   // "Stone"

console.log(person1.sayName == person2.sayName);  // true
```
此例子有缺陷，因为共享了内存。

若是原型链中存在引用类型，那么一个值改变，所有值都会跟着改变。

## 理解原型对象

在默认情况下，所有原型对象都会自动获得一个 constructor（构造函数）属性，这个属性包含一个指向 prototype 属性所在函数的指针（__potor__）。就拿前面的例子来说，Person.prototype.constructor 指向 Person。
而通过这个构造函数，我们还可继续为原型对象添加其他属性和方法。

此时，我们可以通过对象实例访问保存在原型中的值，但却不能通过对象实例重写原型中的值。
如果我们在实例中添加了一个属性，而该属性与实例原型中的一个属性同名，那我们就在实例中创建该属性，该属性将会屏蔽原型中的那个属性。

这就是js的屏蔽方法。
当为对象实例添加一个属性时，这个属性就会屏蔽原型中保存的同名属性

## 更简单的原型语法

为了摆脱每添加一个属性和方法就要敲一遍 `Person.prototype`
为了减少不必要的输入，也为了从视觉上更好地封装原型的功能
常见的做法是用一个包含所有属性和方法的对象字面量来重写整个原型对象
```
function Person(){}

Person.prototype = {
    name : "Stone",
    age : 28,
    job: "Software Engineer",
    sayName : function () {
        console.log(this.name);
    }
};
```

注意：此时 `constructor` 属性不再指向 Person 了。尽管仍能使用。

解决办法是把它加回去`constructor : Person`

注意，以这种方式重设 constructor 属性会导致它的 [[Enumerable]] 特性被设置为 true。
默认情况下，原生的 constructor 属性是不可枚举的，因此如果你使用兼容 ECMAScript 5 的 JavaScript 引擎，可以试一试 Object.defineProperty()。

```
function Person(){}

Person.prototype = {
    name : "Stone",
    age : 28,
    job : "Software Engineer",
    sayName : function () {
        console.log(this.name);
    }
}; 

// 重设构造函数，只适用于 ECMAScript 5 兼容的浏览器
// Object.defineProperty(obj, prop, descriptor)
// 直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。
Object.defineProperty(Person.prototype, "constructor", {
    enumerable: false,
    value: Person
});
```
obj: 要在其上定义属性的对象。
prop: 要定义或修改的属性的名称。
descriptor: 将被定义或修改的属性描述符。

## 原型的动态性

由于在原型中查找值的过程是一次搜索，因此我们对原型对象所做的任何修改都能够立即从实例上反映出来，即使是先创建了实例后修改原型也照样如此。
```
var friend = new Person();

Person.prototype.sayHi = function(){
    console.log("hi");
};

friend.sayHi();   // "hi"（没有问题！）
```
其原因可以归结为实例与原型之间的松散连接关系。
因为实例与原型之间的连接只不过是一个指针，而非一个副本.


但如果是重写整个原型对象，那么情况就不一样了。
我们知道，调用构造函数时会为实例添加一个指向最初原型的[[Prototype]]的 `__potoy__` 指针，而把原型修改为另外一个对象就等于切断了构造函数与最初原型之间的联系。

>请记住：实例中的指针仅指向原型，而不指向构造函数。

## 原型对象的问题

原型中所有属性是被很多实例共享的，对于包含引用类型值的属性来说，问题很突出

```
function Person(){}

Person.prototype = {
    constructor: Person,
    name : "Stone",
    age : 28,
    job : "Software Engineer",
    friends : ["ZhangSan", "LiSi"],
    sayName : function () {
        console.log(this.name);
    }
};

var person1 = new Person();
var person2 = new Person();

person1.friends.push("WangWu");

console.log(person1.friends);    // "ZhangSan,LiSi,WangWu"
console.log(person2.friends);    // "ZhangSan,LiSi,WangWu"
console.log(person1.friends === person2.friends);  // true
```

>引用类型单独占一块堆内存,连原型也只是指针指向数组

## 构造函数和原型结合

#### 构造函数用于定义实例属性，而原型用于定义方法和共享的属性。
结果，每个实例都会有自己的一份实例属性的副本，但同时又共享着对方法的引用，最大限度地节省了内存。
下面的代码重写了前面的例子

```
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.friends = ["ZhangSan", "LiSi"];
}

Person.prototype = {
    constructor : Person,
    sayName : function(){
        console.log(this.name);
    }
}

var person1 = new Person("Stone", 28, "Software Engineer");
var person2 = new Person("Sophie", 29, "English Teacher");

person1.friends.push("WangWu");
console.log(person1.friends);    // "ZhangSan,LiSi,WangWu"
console.log(person2.friends);    // "ZhangSan,LiSi"
console.log(person1.friends === person2.friends);    // false
```
构造函数与原型混成的模式，是目前在 JavaScript 中使用最广泛、认同度最高的一种创建自定义类型的方法。可以说，这是用来定义引用类型的一种默认模式。

## __proto__

当调用构造函数创建一个新实例后，该实例的内部将包含一个指针 __proto__ , 指向构造函数的原型。
```
function Person(){}
var person = new Person();
console.log(person.__proto__ === Person.prototype); // true
```

Object.__proto__ = Object.prototype;

## 原型链
JavaScript 中描述了原型链的概念，并将原型链作为实现继承的主要方法。
其基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。

简单回顾一下构造函数、原型和实例的关系：

每个构造函数都有一个原型对象(调用对象)
原型对象都包含一个指向构造函数的指针(prototype)
而实例都包含一个指向原型对象的内部指针。(__proto__)

