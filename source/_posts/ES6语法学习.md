---
title: ES6语法学习
date: 2018-04-13 15:39:19
categories:
  - JS学习笔记
tags:
  - ES6
---

# ES6

在代码中，如果使用了ES6语法，那么强制开启严格模式

## let和const

- let:使用let定义的变量，拥有块级作用域.

注意：使用let定义变量，在同一块级作用域内，不能重复定义同一个名字。

<!-- more -->

- const:使用const定义的是常量，不可更改，且也拥有块级作用域。

注意：const只能在定义赋值，因为它定义的是常量，不可更改。除非在定义时赋值的常量，是一个Object。因为Object是引用类型，赋值Object等于给const赋值了一个指针，指针不会变，Object改变不影响指向它的指针。

### 涉及的JS基础知识

基本类型（私人空间）：每定义一个变量，就会在栈内存中开辟一个存放这个变量的空间。

引用类型（公共空间）：如果有两个不同的变量指向同一个对象，则这个对象是唯一存在堆内存的。当复制保存着对象的某个变量时，操作的是对象的引用，但在为对象添加属性时，操作的是实际的对象。

## 解构赋值

示例：
数组解构赋值

    {
        let a,b;
        [a,b]=[1,2];
        console.log(a,b); // 1 2
    }

赋值展开平铺语法：...变量

    {
        let a,b,rest;
        [a,b,...rest]=[1,2,3,4,5,6];
        console.log(a,b,rest); // 1 2 [3,4,5,6]
    }

对象解构赋值：

    {
        let a,b;
        ({a,b}={a:1,b:2})
        console.log(a,b); // 1 2
    }

如果解构赋值没有在结构上成功配对，则赋值对象为未定义,否则设定默认值

    {
        let a,b,c,rest;
        [a,b,c=3]=[1,2]
        console.log(a,b,c) //1 2 3
    }

解构赋值的常用使用场景：
1.数值交换

    {
        let a=1;
        let b=2;
        [a,b]=[b,a];
        console.log(a,b); // 2 1
    }

2.函数返回

    {
        function f(){
            return [1,2]
        }
        let a,b;
        [a,b]=f();
        console.log(a,b); // 1 2
    }

3.函数选择返回

    {
        function f(){
            return [1,2,3,4,5,]
        }
        let a,b,c;
        [a,,,b]=f();
        console.log(a,b); // 1 4
    }

4.函数平铺返回

    {
        function f(){
            return [1,2,3,4,5,]
        }
        let a,b,c;
        [a,...b]=f();
        console.log(a,b); // 1 [2,3,4,5]
    }

5.对象解构赋值

    {
        let o={p:42,q:true};
        let {p,q}=o;
        console.log(p,q); // 42 true
    }

6,对象设置默认值

    {
        let {a=10,b=5}={a:3}
        console.log(a,b); // 3 5
    }

### 解构赋值：模拟后台json数据

    {
        let metaData={
            title:'abc',
            test:[{
                title:'test',
                desc:'description'
            }]
        }
        let {title:esTitle,test:[{title:cnTitle}]}=metaData;
        console.log(esTitle,cnTitle);
    }

## Module模块化

导出：
export xxx()
export default {
    xxx,
    zzz
    yyy
}

引入：
import xxx from './路径'
