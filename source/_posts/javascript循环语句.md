---
title: javascript循环语句
date: 2018-04-28 00:07:03
categories:
  - JS学习笔记
tags:
  - 循环语句 js
---
[TOC]

学习和记录循环语句需要注意的重要细节，深入理解js的循环语句

## for（条件循环）

一般for循环的语法为：
```
for(语句1;语句2;语句3){
    被执行的代码块；
}
```

语句 1 在循环（代码块）开始前执行 
语句 2 定义运行循环（代码块）的条件 
语句 3 在循环（代码块）已被执行之后执行

 <!-- more -->

干巴巴的说明有点无聊，不方便记忆，我们来做一道经典题型

以面试题级别的题型为例：

```
// 挑战一
var k;
for(i=0, j=0; i<10, j<6; i++, j++){
    k = i + j;
}
console.log(k);  // ？？？
```
偶不！它一次定义了很多条语句！这不是for循环！
等等……仔细一看，它只有三个分号。冷静，这是我们刚才学习的for循环！

毕竟也没有谁规定一个语句之中，只能写一个判断，这样做是可以的。
那么，答案会是多少呢？

一开始，i和j都等于零。
条件判断后，i和j都自加一。

但条件判断是以谁为基准？分别循环？不可能！

所幸，我们不是真的在面试，直接用浏览器运行便能一窥究竟。
答案是10

而如果把i<10 和 j<6 的位置对调
则会输出18

>由此可见，for循环的条件判断`停止条件`，由最接近第二个分号的条件为准。

- 顺便一提，当循环遍历一个数组，并且数组的长度在循环过程中不会改变时，我们应将数组长度用变量存储起来，这样会获得更好的效率

例：
>const arr = [1, 2, 3];
for(let i = 0, len = arr.length; i < len; i++)

----

## for-in (遍历所有可枚举的属性)

for-in语句用于对数组或者对象的`属性`进行循环操作。

for-in 循环中的代码每执行一次，就会对`数组的元素`或者`对象的属性`进行一次操作。

Tip:
>for-in循环应该用在`非数组对象`的遍历上，使用for-in进行循环也被称为“枚举”。因为它真正遍历的是对象`“可枚举的属性”`

注意：
>for-in 遍历属性的顺序并不确定。
即输出的结果顺序与属性在对象中的顺序无关，也与属性的字母顺序无关，与其他任何顺序也无关。

### Array 的真相

Array 在 Javascript 中是一个对象， Array 的索引是属性名。

事实上， Javascript 中的 “array” 有些误导性， Javascript 中的 Array 并不像大部分其他语言的数组。首先， Javascript 中的 Array 在内存上并不连续，其次， Array 的索引并不是指偏移量。

实际上， Array 的索引也不是 Number 类型，而是 String 类型的。我们可以正确使用如 arr[0] 的写法的原因是语言可以自动将 Number 类型的 0 转换成 String 类型的 “0″ 。所以，在 Javascript 中从来就没有 Array 的索引，而只有类似 “0″ 、 “1″ 等等的属性。

有趣的是，每个 Array 对象都有一个 length 的属性，导致其表现地更像其他语言的数组。但为什么在遍历 Array 对象的时候没有输出 length 这一条属性呢？

那是因为 for-in 只能遍历“可枚举的属性”， length 属于不可枚举属性，实际上， Array 对象还有许多其他不可枚举的属性。


现在我们来看看用 for-in 来循环数组的特殊例子

```
const arr = [1, 2, 3];
arr.name = "Hello world";
let index;
for(index in arr) {
    console.log("arr[" + index + "] = " + arr[index]);
}
//运行结果
//arr[0] = 1
//arr[1] = 2
//arr[2] = 3
//arr[name] = Hello world
```
我们看到 for-in 循环访问了我们新增的 “name” 属性，因为 for-in 遍历了对象的所有属性，而不仅仅是“索引”。

同时需要注意的是，此处输出的索引值，即 “0″、 “1″、 “2″不是 Number 类型的，而是 String 类型的，因为其就是作为属性输出，而不是索引。

那是不是说，只要我们不在 Array 对象中添加新的属性，我们就可以只输出数组中的内容了呢？答案是否定的。

- 因为 for-in 不仅仅遍历 array 自身的属性，它还遍历 array 原型链上的所有可枚举的属性。


下面我们看个例子：

```
Array.prototype.fatherName = "Father";
const arr = [1, 2, 3];
arr.name = "Hello world";
let index;
for(index in arr) {
    console.log("arr[" + index + "] = " + arr[index]);
}
//arr[0] = 1
//arr[1] = 2
//arr[2] = 3
//arr[name] = Hello world
//arr[fatherName] = Father
```
写到这里，我们可以发现 for-in 并不适合用来遍历 Array 中的元素，其更适合遍历对象中的属性，这也是其被创造出来的初衷。

- 但是有一种情况例外，那就是稀疏数组。

```
let key;
const arr = [];
arr[0] = "a";
arr[100] = "b";
arr[10000] = "c";
for(key in arr) {
    if(arr.hasOwnProperty(key) && 
    /^0$|^[1-9]\d*$/.test(key) && 
    key <= 4294967294 
    ) {
     console.log(arr[key]);
    }
}
```
- for-in 只会遍历存在的实体.

上面的例子中， for-in 遍历了3次（遍历属性分别为”0″、 “100″、 “10000″的元素，而普通 for 循环则会遍历 10001 次）。
所以，只要处理得当， for-in 在遍历 Array 中元素也能发挥巨大作用。


for-in 性能
正如上面所说，每次迭代操作会同时搜索实例或者原型属性， for-in 循环的每次迭代都会产生更多开销，因此要比其他循环类型慢，一般速度为其他类型循环的 1/7。
因此，除非明确需要迭代一个属性数量未知的对象，否则应避免使用 for-in 循环。

如果需要遍历一个数量有限的已知属性列表，使用其他循环会更快，比如下面的例子：
```
const obj = {
 "prop1": "value1",
 "prop2": "value2"
};
  
const props = ["prop1", "prop2"];
for(let i = 0; i < props.length; i++) {
 console.log(obj[props[i]]);
}
```
上面代码中，将对象的属性都存入一个数组中，相对于 for-in 查找每一个属性，该代码只关注给定的属性，节省了循环的开销和时间。

#### for-in总结：

最后来一个面试题

```
var nums = [12,32,54,56,78,89];
for(var n in nums){
    console.log(n);  // 0,1,2,3,4,5
}
```
因为for-in的真正作用已经解释了很多，所以我们只需要梳理一下知识点，即可轻松解答


总结：
①：Array实际上不存在索引（但存在类似的属性）
②：for-in只能遍历可枚举属性（几乎所有属性）
③：for-in并不直接获取数组元素（获取元素方法：数组[索引属性]）
④：for-in输出顺序不确定（不会自动排列）

由此可得：
①：console.log(n)不可能输出元素
②：n是索引属性

- 所以，Array不存在索引，但for-in遍历索引属性（普通的for循环是遍历数组长度）。

补充：（过滤不想要的属性）
```
for(var i in a) {
    // 跳过继承的属性
    if (!a.hasOwnProperty(i)) continue; // continue结束本次循环

    // 跳过不是非负整数的 i
    if (String(Math.floor(Math.abs(Number(i)))) !== i) continue;
}
```

----

## forEach (遍历所有元素并回调)

在 ES5 中，引入了新的循环，即 forEach 循环。

```
const arr = [1, 2, 3];
arr.forEach((data) => {
    console.log(data);
});
//1
//2
//3
```
forEach 方法为数组中含有有效值的每一项执行一次 callback 函数。
那些已删除（使用 delete 方法等情况）或者从未赋值的项将被跳过（不包括那些值为 undefined 或 null 的项）。 

callback 函数会被依次传入三个参数：

forEach(值,索引,数组对象本身)，默认只遍历值

- 数组当前项的值；
- 数组当前项的索引；
- 数组对象本身；

需要注意的是，forEach 遍历的范围在第一次调用 callback 前就会确定。
调用forEach 后添加到数组中的项不会被 callback 访问到。

如果已经存在的值被改变，则传递给 callback 的值是 forEach 遍历到他们那一刻的值。
已删除的项不会被遍历到。

```
const arr = [];
arr[0] = "a";
arr[3] = "b";
arr[10] = "c";
arr.name = "Hello world";
arr.forEach((data, index, array) => {
    console.log(data, index, array);
});
```

运行结果:

```
a 0 ["a", 3: "b", 10: "c", name: "Hello world"]
b 3 ["a", 3: "b", 10: "c", name: "Hello world"]
c 10 ["a", 3: "b", 10: "c", name: "Hello world"]
```

这里的 index 是 Number 类型，并且也不会像 for-in 一样遍历原型链上的属性。

所以，使用 forEach 时，我们不需要专门地声明 index 和遍历的元素，因为这些都作为回调函数的参数。

另外，forEach 将会遍历数组中的所有元素，但是 ES5 定义了一些其他有用的方法，下面是一部分：
- every: 循环在第一次 return false 后返回
- some: 循环在第一次 return true 后返回
- filter: 返回一个新的数组，该数组内的元素满足回调函数
- map: 将原数组中的元素处理后再返回
- reduce: 对数组中的元素依次处理，将上次处理结果作为下次处理的输入，最后得到最终结果。


forEach 性能：一般
forEach 的速度不如 for ，因为forEach会遍历所有元素，并一一进行回调。

