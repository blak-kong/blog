---
title: js函数理解
date: 2018-04-30 16:43:47
categories:
  - JS学习笔记
tags:
  - 函数 js
---
[toc]

# 函数

函数是一段代码，它只定义一次，但可以被执行或调用任意次。
在 JavaScript 里，函数即对象，程序可以随意操控它们。
（万物皆对象）

## 函数定义

在 JavaScript 中，函数实际上是对象，每个函数都是 Function 构造函数的实例，因此函数名实际上也是一个指向函数对象的指针，不会与某个函数绑定。

函数通常有以下3中定义方式。例如：

<!-- more -->

```
// 写法一：函数声明（推荐写法）
function sum (num1, num2) {
    return num1 + num2;
}

// 写法二：函数表达式（推荐写法）
var sum = function(num1, num2){
    return num1 + num2;
};

// 写法三：Function 构造函数（不推荐写法）
var sum = new Function("num1", "num2", "return num1 + num2"); 
```

>由于函数名仅仅是指向函数的指针，因此函数名与包含对象指针的其他变量没有什么不同。

换句话说，一个函数可能会有多个名字。
例如：

```
function sum(num1, num2){
    return num1 + num2;
}
console.log(sum(10,10));        // 20

var anotherSum = sum;
console.log(anotherSum(10,10)); // 20

sum = null;
console.log(anotherSum(10,10)); // 20
```
## 没有重载--函数名是指针

将函数名想象为指针，也有助于理解为什么 JavaScript 中没有函数重载的概念。
(因为改变函数体内容，只是指向改变，内存并没有清空)
```
function addSomeNumber(num){
    return num + 100;
}

function addSomeNumber(num, num2) {
    return num + 200;
}

var result = addSomeNumber(100);    // 300
```
显然，这个例子中声明了两个同名函数，而结果则是后面的函数覆盖了前面的函数。
以上代码实际上与下面的代码没有什么区别：
(因为改变函数体内容，只是指向改变，内存并没有清空)
```
var addSomeNumber = function (num){
    return num + 100;
};

addSomeNumber = function (num, num2) {
    return num + 200;
};

var result = addSomeNumber(100);    // 300
```

## 函数声明与函数表达式--变量提升

解析器在向执行环境中加载数据时，对「函数声明」和「函数表达式」并非一视同仁。
解析器会率先读取函数声明，并使其在执行任何代码之前可用（可以访问）；----变量提升
至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被解释执行。

例如：
```
console.log(sum(10,10)); // 20
function sum(num1, num2){
    return num1 + num2;
}
```
以上代码完全可以正常运行。因为在代码开始执行之前，解析器就已经通过一个名为函数声明提升（function declaration hoisting）的过程，读取并将函数声明添加到执行环境中。
```
console.log(sum(10,10)); // Uncaught TypeError: sum is not a function
var sum = function(num1, num2){
    return num1 + num2;
};
```

## 作为值的函数

因为 JavaScript 中的函数名本身就是变量，所以函数也可以作为值来使用。
也就是说，不仅可以像传递参数一样把一个函数传递给另一个函数，而且可以将一个函数作为另一个函数的结果返回。来看一看下面的函数。
```
function callSomeFunction(someFunction, someArgument){
    return someFunction(someArgument);
}
```
这个函数接受两个参数。第一个参数应该是一个函数，第二个参数应该是要传递给该函数的一个值。
然后，就可以像下面的例子一样传递函数了。
```
function add10(num){
    return num + 10;
}

var result1 = callSomeFunction(add10, 10);
console.log(result1);   // 20

function getGreeting(name){
    return "Hello, " + name;
}

var result2 = callSomeFunction(getGreeting, "Nicholas");
console.log(result2);   // "Hello, Nicholas"
```

## 函数的形参和实参

在函数内部，有两个特殊的对象：arguments 和 this。
其中，arguments 是一个类数组对象，包含着传入函数中的所有参数。
虽然 arguments 的主要用途是保存函数参数，但这个对象还有一个名叫 callee 的属性，该属性是一个指针，指向拥有这个 arguments 对象的函数。
请看下面这个非常经典的阶乘函数。
```
function factorial(num){
	if (num <= 1) {
        return 1;
    } else {
        return num * factorial(num-1)
    }
}
```

定义阶乘函数一般都要用到递归算法，如上面的代码所示，在函数有名字，而且名字以后也不会变的情况下，这样定义没有问题。
但问题是这个函数的执行与函数名 factorial 紧紧耦合在了一起。
为了消除这种紧密耦合的现象，可以像下面这样使用 arguments.callee
```
function factorial(num){
    if (num <=1) {
        return 1;
    } else {
        return num * arguments.callee(num-1)
    }
}
```
在这个重写后的 factorial() 函数的函数体内，没有再引用函数名 factorial。
这样，无论引用函数时使用的是什么名字，都可以保证正常完成递归调用。
例如：
```
// 这里需要结合上一段代码
var trueFactorial = factorial;

factorial = function(){// 重置了factorial函数指向
    return 0;
};

console.log(trueFactorial(5));  // 120
console.log(factorial(5));      // 0
```
#### arguments用法：使用在函数体中，可用 `arguments.callee` 指向拥有这个 arguments 对象的函数。

## 函数的属性和方法

JavaScript 中的函数是对象，因此函数也有属性和方法。
每个函数都包含两个属性：length 和 prototype。
其中，length 属性表示函数希望接收的命名参数的个数，也就是形参个数。

不过这不重要。

接下来要说的才是属性与方法。
JavaScript中的每一个Function对象都有一个apply()方法和一个call()方法，它们的语法分别为：

```
/*apply()方法*/
function.apply(thisObj[, argArray])

/*call()方法*/
function.call(thisObj[, arg1[, arg2[, [,...argN]]]]);
```

它们各自的定义：

#### apply：调用一个对象的一个方法，用另一个对象替换当前对象。即A对象应用B对象的方法。
>例如：B.apply(A, arguments);即A对象应用B对象的方法。

#### call：调用一个对象的一个方法，用另一个对象替换当前对象。即A对象调用B对象的方法。
>例如：B.call(A, args1,args2);即A对象调用B对象的方法。

它们的共同之处：
都“可以用来代替另一个对象调用一个方法，将一个函数的对象上下文从初始的上下文改变为由thisObj指定的新对象”。

它们的不同之处：

apply：最多只能有两个参数——新this对象和一个数组argArray。如果给该方法传递多个参数，则把参数都写进这个数组里面，当然，即使只有一个参数，也要写进数组里。如果argArray不是一个有效的数组或arguments对象，那么将导致一个TypeError。
如果没有提供argArray和thisObj任何一个参数，那么Global对象将被用作thisObj，并且无法被传递任何参数。

call：它可以接受多个参数，第一个参数与apply一样，后面则是一串参数列表。这个方法主要用在js对象各方法相互调用的时候，使当前this实例指针保持一致，或者在特殊情况下需要改变this指针。如果没有提供thisObj参数，那么 Global 对象被用作thisObj。 

实际上，apply和call的功能是一样的，只是传入的参数列表形式不同。

基本用法
```
function add(a,b){
  return a+b;  
}
function sub(a,b){
  return a-b;  
}
var a1 = add.apply(sub,[4,2]);　　//sub应用add的方法
var a2 = sub.apply(add,[4,2]);
alert(a1);  //6     
alert(a2);  //2

/*call的用法*/
var a2 = add.call(sub,4,2); //sub调用add的方法
alert(a2); //6 
```

## 关卡：
```
// 挑战一，合并任意个数的字符串
var concat = function(){
    var result = '';
    for(var i = 0; i < arguments.length; i ++){ // 遍历arguments实参长度
        result += arguments[i]; // 把所有实参元素累加
    }
    return result;
}
console.log(concat('st','on','e'));  // stone
```
