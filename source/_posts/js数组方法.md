---
title: js数组方法
date: 2018-04-29 16:55:21
categories:
  - JS学习笔记
tags:
  - 数组 js
---
[toc]

## 使用数组字面量创建数组（推荐）
```
var empty = [];
```
## 使用 `new` 关键字创建数组

使用 new 关键字调用构造函数 Array() 是创建数组的另一种方法。
我们可以用三种方式调用构造函数。例如：
<!-- more -->
```
// 调用时没有参数
var a = new Array();

// 调用时有一个数值参数，它指定长度
var a = new Array(10); 

// 显式指定多个数组元素或者数组的一个非数值元素
var a = new Array(5, 4, 3, 2, 1, "testing");
```

## 稀疏数组

#### 稀疏数组就是包含从0开始的不连续索引的数组。

## 数组遍历

经典for循环

```
var keys = Object.keys(o);   // 获得 o 对象属性名组成的数组
var values = []              // 在数组中存储匹配属性的值
for(var i = 0; i < keys.length; i++) {  // 对于数组中每个索引
    var key = keys[i];                  // 获得索引处的键值
    values[i] = o[key];                 // 在 values 数组中保存属性值
}
```

for-in 循环能够枚举继承的属性名，如添加到 Array.prototype 中的方法。
forEach(值,索引,数组对象本身) 循环所有元素并回调，传入的参数仅表示是否回调
具体案例参考另一篇博客，[javascript循环语句](https://blak-kong.github.io/2018/04/28/javascript%E5%BE%AA%E7%8E%AF%E8%AF%AD%E5%8F%A5/#forEach-遍历所有元素并回调)
 

## 数组检测

#### `Array.isArray()` es5语法：给定一个未知的对象，判定它是否为数组。
 

## 数组转换

转换方法`toString()`：当调用数组的 toString() 方法，会返回以逗号分隔数组中每个值的字符串。为了创建这个字符串会调用数组每一项的 toString() 方法。

转换方法`toLocaleString()`：基本同上。但内置本地字符串格式转换功能（时间格式转换）。
```
var sd=new Date()
sd.toLocaleString() //"2017/2/15 上午11:21:31"
sd.toString() //"Wed Feb 15 2017 11:21:31 GMT+0800 (CST)"
```


#### `join()` 可以使用不同的分隔符来构建字符串数组。

```
var colors = ["red", "green", "blue"];
console.log(colors.join(","));    // red,green,blue
console.log(colors.join("||"));   // red||green||blue
```

#### `split()` 用于把一个字符串分割成字符串数组。用于把一个字符串分割成字符串数组。
第一个参数是指定字符串或正则表达式。若传入第二个参数，可以指定数组长度
```
"2:3:4:5".split(":")	//将返回["2", "3", "4", "5"]
"|a|b|c".split("|")	    //将返回["", "a", "b", "c"]
```

## 栈方法和队列方法

### 栈方法（后进先出）：`push()` 和 `pop()` 
// 越后面加入数组的，越先被`pop()`移除
`push()` 把任意数量参数逐个添加到数组末尾，并返回修改后数组的长度。
`pop()` 从数组末尾移除最后一项，减少数组的 length 值，然后返回移除的项。

```
var colors = [];                            // 创建一个数组
var count = colors.push("red", "green");    // 推入两项
console.log(count);                         // 2，数组的长度

count = colors.push("black");               // 推入另一项
console.log(count);                         // 3，数组的长度

var item = colors.pop();                    // 取得最后一项
console.log(item);                          // "black",返回移除的项
console.log(colors.length);                 // 2，数组的长度
```

### 队列方法（先进先出）`push()` 和 `shift()`

`push()` 把任意数量参数逐个添加到数组末尾，并返回修改后数组的长度。
`shift()` 从数组前端移除第一项，减少数组的 length 值，然后返回移除的项。

### 反向队列方法（反方向先进先出）`unshift()`和`pop()` 

`unshift()` 把任意数量参数逐个添加到数组前端，然后返回length 值
`pop()` 从数组末尾移除最后一项，减少数组的 length 值，然后返回移除的项。

## 重排序方法

数组中有两个重排序的方法：reverse() 和 sort()。

#### reverse() 方法可以反转数组元素的顺序。

#### sort() 方法可以按升序或降序排列数组元素。

sort具体用法，可以查看[深入理解js对象排序-sort()](https://blak-kong.github.io/2018/04/28/%E6%B7%B1%E5%85%A5%E6%B5%85%E5%87%BAjs%E5%AF%B9%E8%B1%A1%E6%8E%92%E5%BA%8F/#more)

通用代码演示
```
var arr = [10, 20, 1, 2];

arr.sort(function (x, y) {
    if (x < y) {
        return -1;
    }
    if (x > y) {
        return 1;
    }
    return 0;
});
console.log(arr); // [1, 2, 10, 20]
```

## 数组操作

#### `concat()` 基于当前数组中的所有项创建一个新数组。

这个方法会先创建当前数组一个副本，然后将接收到的参数添加到这个副本的末尾，最后返回新构建的数组。

```
var a = ["red", "green", "blue"];
var b = a.concat("yellow", ["black", "brown"]);//基于a创建新数组

console.log(a);     // red,green,blue
console.log(b);    // red,green,blue,yellow,black,brown
```

#### `slice()` 它能够基于当前数组中的一或多个项创建一个新数组。

slice() 方法可以接受一或两个参数，即要返回项的起始和结束位置。

在只有一个参数的情况下，slice() 方法返回从该参数指定位置开始到当前数组末尾的所有项。
如果有两个参数，该方法返回起始和结束位置之间的项，但不包括结束位置的项。

- 注意，slice() 方法不会影响原始数组。

```
var a = ["red", "green", "blue", "yellow", "purple"];
var a1 = a.slice(1);
var a2 = a.slice(1,4);

console.log(a1);   // green,blue,yellow,purple
console.log(a2);   // green,blue,yellow
```

#### `splice()` 它的主要用途是向数组的中部插入元素,或者进行删除、替换

写法：Array.splice(起始位置，删除数量，插入)

只写前两项，是从指定位置开始删除
不写第二项，写第三项，是插入元素
三个都写，则是替换指定位置的元素

## 位置方法

ECMAScript 5 为数组实例添加了两个位置方法：
indexOf() 和 lastIndexOf()。
这两个方法都接收两个参数：要`查找的项`和（可选的）`查找起点位置的索引`

#### `indexOf()` 方法从数组的开头（位置0）开始向后查找

#### `lastIndexOf()` 方法则从数组的末尾开始向前查找。

注意：最终返回的索引，都是数组开头往后算。

这两个方法都返回要查找的项在数组中的位置，或者在没找到的情况下返回 -1。
它们在比较第一个参数与数组中的每一项时，会使用全等操作符；
也就是说，要求查找的项必须严格相等（就像使用 === 一样）。

例如：
```
var numbers = [1,2,3,4,5,4,3,2,1];
console.log(numbers.indexOf(4));          // 3 从前往后，查找数组第一个4
console.log(numbers.lastIndexOf(4));      // 5 从后往前，查找数组第一个4
console.log(numbers.indexOf(4, 4));       // 5 从前往后，从索引4开始查找元素4
console.log(numbers.lastIndexOf(4, 4));   // 3 从后往前，从索引-4开始往前查找元素4

var person = { name: "Nicholas" };
var people = [{ name: "Nicholas" }];
var morePeople = [person];
console.log(people.indexOf(person));      // -1  没找到 -1
console.log(morePeople.indexOf(person));  // 0   找到索引 0
```

## 迭代方法

ECMAScript 5 为数组定义了5个迭代方法。

每个方法都接收两个参数：要在每一项上运行的`函数`和（可选的）`运行该函数的作用域对象`。
传入这些方法中的函数会接收三个参数：`值`、`索引`和`数组对象本身`。
根据使用的方法不同，这个函数执行后的返回值可能会也可能不会影响访问的返回值。

通用使用方法

- array1.xxxx(callbackfn[, thisArg])

array1
必需。一个数组对象。

callbackfn
必需。一个接受最多三个参数的函数。对于数组中的每个元素，filter 方法都会调用 callbackfn 函数一次。

thisArg
可选。可在 callbackfn 函数中为其引用 this 关键字的对象。如果省略 thisArg，则 undefined 将用作 this 值。

以下是这5个迭代方法的作用。

- `every()` 对数组中的每一项运行给定函数，如果该函数对每一项都返回 true ，则返回 true。
- `filter()` 过滤器：对数组中的每一项运行给定函数，返回该函数会返回 true 的项组成的数组。
- `forEach()` 元素遍历：对数组中的每一项运行给定函数。这个方法没有返回值。
- `map()` 数组映射：对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
- `some()` 对数组中的每一项运行给定函数，如果该函数对任一项返回 true ，则返回 true。

5个迭代方法的用法：

#### `every()` 确定数组的所有成员是否满足指定的测试。

array1.every(callbackfn[, thisArg])

- 判断数组元素是否全部都能整除2

```
// 定义回调函数
function CheckIfEven(value, index, ar) { // CheckIfEven(值，索引，数组本身)

    if (value % 2 == 0) { // 取余，检查是否能整除2
        return true;
    } else {
        return false;
    }
}

// 创建数组 Create an array.
var numbers = [2, 4, 5, 6, 8];

// 检查回调函数是否返回所有的数组值
if (numbers.every(CheckIfEven)) {
        console.log("All are even."); // true,满足指定的测试时运行
    } else {
        console.log("Some are not even."); // false,不满足指定的测试时运行
    }

// 输出(Output):  Some are not even.
```

#### `filter()` 返回数组中的满足回调函数中指定的条件的元素

array1.filter(callbackfn[, thisArg])

- 以下代码为，使用过滤器判断数组中的质数

```
// 定义回调函数
function CheckIfPrime(value, index, ar) {
    // Math.floor向下取整
    // Math.sqrt() 方法可返回一个数的平方根。
    high = Math.floor(Math.sqrt(value)) + 1; // 平方根向下取整再加一

    for (var div = 2; div <= high; div++) { // 判断是否能否被2以上的数整除
        if (value % div == 0) { // 如果能被整除，则不是质数
            return false;
        }
    } 
    return true;
}

// 创建初始数组
var numbers = [31, 33, 35, 37, 39, 41, 43, 45, 47, 49, 51, 53];

// 获取原始数组中的质数
var primes = numbers.filter(CheckIfPrime);

console.log(primes);  // [31,37,41,43,47,53]
```

#### `forEach()` 为数组中的每个元素执行指定操作。

array1.forEach(callbackfn[, thisArg])

若不写回调函数，则默认操作为遍历所有元素。

- 以下代码为，数组求和

```
// 创建数组
var numbers = [10, 11, 12];

// 调用每个数组元素，并回调函数
var sum = 0;
numbers.forEach(
    function addNumber(value) { sum += value; } // 所有元素加起来
);

console.log(sum);  // 输出(Output): 33
```

#### `map()` 对数组的每个元素调用定义的回调函数，并返回包含结果的数组。

array1.map(callbackfn[, thisArg])

- 以下代码为，数组取余

```
// 定义一个包含取余属性的对象
// 原型方法定义
var obj = {
    divisor: 10, // 除以10
    remainder: function (value) {
        return value % this.divisor;
    }
}

// 创建数组
var numbers = [6, 12, 25, 30];

// 获取余数
var result = numbers.map(obj.remainder, obj);
console.log(result);

// 输出(Output):  [6,2,5,0]
```

#### `some()` 确定指定的回调函数是否为数组中的任何元素均返回 true。

array1.some(callbackfn[, thisArg])

- some()判断是否存在至少一个数符合条件

```
// 创建一个函数，如果值为true，则返回true。
var isOutsideRange = function (value) {
    // 检测是否超出范围
    return value < this.minimum || value > this.maximum;
}

// 创建数组.
var numbers = [6, 12, 16, 22, -12];

// 范围对象是“这个”对象。
var range = { minimum: 10, maximum: 20 };

console.log(numbers.some(isOutsideRange, range));  // true 存在小于10和大于20的数
```

## 挑战 数组去重

// 挑战一，一维数组

- 思路
使用indexOf(i)检测新数组newArr里有没有包含Arr里的i项
如果没有则向newArr里添加Aii[i]项
如果有则跳过；不做任何操作。

```
var arr = [2,3,4,2,3,5,6,4,3,2];
var unique = function(arr){
    var result = []; // 创建空数组，用于存放新数组
    arr.forEach(function(item){

        // indexOf()会使用全等操作符,判断item索引与遍历数组中所有元素都不相等，才运行
        if(result.indexOf(item) < 0){

        // 此处代码，只有数组索引小于0（数组中不存在），才会运行
        // 所以可以达到去重效果
            result.push(item);
        }
    });
    return result;
}
console.log(unique(arr)); // [2,3,4,5,6]
```

// 挑战二（二维数组）
```
var arr = [2,3,4,[2,3,4,5],3,5,[2,3,4,2],4,3,6,2];
var unique = function(arr){
    var result = []; // 创建空数组，用于存放新数组
    arr.forEach(function(item) { // 遍历数组元素给item
        if (Array.isArray(item)) { // 判定item是否为数组，只有数组才能往下运行
            item.forEach(function(i) { // 把item中的数组，遍历给i
                // indexOf()会使用全等操作符,判断i索引与遍历数组中所有元素都不相等，才能往下运行
                if (result.indexOf(i) < 0) {
                    result.push(i);
                }
            });
        } else {
            if (result.indexOf(item) < 0) {
                result.push(item);
            }
        }
    });
    return result;
}
console.log(unique(arr)); // [2,3,4,5,6]
```

