---
title: ajax学习
date: 2018-05-01 14:46:04
categories:
  - JS学习笔记 vue学习
tags:
  - ajax 网络请求 js
---

[toc]

# ajax
 

## XMLHttpRequest 是 AJAX 的基础。

- XMLHttpRequest 对象

XMLHttpRequest 是一个 API，它为客户端提供了在客户端和服务器之间传输数据的功能。
它提供了一个通过 URL 来获取数据的简单方式，并且不会使整个页面刷新。
这使得网页只更新一部分页面而不会打扰到用户。XMLHttpRequest 在 AJAX 中被大量使用。

>通过它，你可以很容易的取回一个 URL 上的资源数据。

尽管名字里有 XML，但 XMLHttpRequest 可以取回所有类型的数据资源，并不局限于 XML。
而且除了 HTTP ，它还支持 file 和 ftp 协议。
<!-- more -->

## 构造函数 XMLHttpRequest()

构造函数初始化一个 XMLHttpRequest 对象。必须在所有其他方法被调用前调用构造函数。

语法：
>var myRequest = new XMLHttpRequest();

此时 myRequest 已经成为一个XMLHttpRequest 对象，可以使用XMLHttpRequest的方法。


## 向服务器发送请求
如需将请求发送到服务器，我们使用 XMLHttpRequest 对象的 open() 和 send() 方法：
```
xmlhttp.open("GET",url,true);
xmlhttp.send(null);
```

- open(method,url,async) // 	
规定请求的类型、URL 以及是否异步处理请求。
    - method：请求的类型；GET 或 POST
    - url：文件在服务器上的位置
    - async：true（异步）或 false（同步）

- send(string) // 将请求发送到服务器。
    - string：仅用于 POST 请求（如果不需要发送请求，则必须传入Null）

## GET请求

如果您希望通过 GET 方法发送信息，请向 URL 添加信息（在地址后面加问号，然后再添加）：
```
xmlhttp.open("GET","/try/ajax/demo_get2.php?fname=Henry&lname=Ford",true);
xmlhttp.send();
```

## POST请求

如果需要像 HTML 表单那样 POST 数据，请使用 setRequestHeader() 来添加 HTTP 头。
然后在 send() 方法中规定您希望发送的数据：
```
xmlhttp.open("POST","/try/ajax/demo_post2.php",true);
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
xmlhttp.send("fname=Henry&lname=Ford");
```
方法:
>setRequestHeader(header,value)

- 向请求添加 HTTP 头。
    - header: 规定头的名称
    - value: 规定头的值

## 我们使用 GET 还是 POST？

GET 更简单也更快，并且在大部分情况下都能用。
然而，在以下情况中，请使用 POST 请求：
- 无法使用缓存文件（更新服务器上的文件或数据库）
- 向服务器发送大量数据（POST 没有数据量限制）
- 发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

## 异步如何设置 - True 或 False？

AJAX 指的是异步 JavaScript 和 XML（Asynchronous JavaScript and XML）。
XMLHttpRequest 对象如果要用于 AJAX 的话，其 open() 方法的 async 参数必须设置为 true。

通过 AJAX，JavaScript 无需等待服务器的响应，而是：
- 在等待服务器响应时执行其他脚本
- 当响应就绪后对响应进行处理

## 接收响应

一个完整的http响应是由状态码，响应头集合，和响应主题组成。
在收到响应的消息后，这些都是可以通过xhr对象的属性和方法所使用。
它们主要有以下4个属性

```
responseText;作为相应主题返回的文本，（文本形式）
responseXML;如果响应的内容是‘text/xml’或者是哦application/xml;属性中将会保存，响应数据的xml形式。DOM文档形式。

status: http的状态码（数字形式）
statusText;http状态说明，（文本形式）
```

## ajax原生

```
//**********第一步, 获得一个xhr对象*************

       var xmlHttpReq = null;// 声明一个空对象用来装入XMLHttpRequest

       if (window.XMLHttpRequest){// ie7 以上的浏览器XMLHttpRequest是window的子对象

              xmlHttpReq = new XMLHttpRequest();// 实例化一个XMLHttpRequest

       }else (window.ActiveXObject){// IE5 IE6是以ActiveXObject的方式引入XMLHttpRequest的

              xmlHttpReq = new ActiveXObject("Microsoft.XMLHTTP");

       }

       if(xhr != null){  // 如果对象实例化成功
              //设置回调函数
              xhr.onreadystatechange = function(){

                  if(xhr.readyState == 4){  // 确定响应已经成功返回
                       // 200可作为成功标志, 304表示请求资源没有修改, 可直接使用浏览器缓存
                       if ((xhr.status>=200 && xhr.status < 300 ) || xhr.status == 304){
                             alert(xhr.responseText); // 请求成功，服务器返回的数据
                        } else {
                             alert( "请求失败: " + xhr.status);
                        }
                    }
              }

//************第二步: 启动请求.******************
              // open方法接收三个参数: 要发送的请求类型(get,post等), 请求的url和是否异步发送请求的布尔值
              xhr.open("get","test.php",true);
              // 调用open()方法并采用异步方式. 如果第三个参数是false, 同步执行, 则js代码会等到服务器响应之后再继续执行

//*************第三步: 发送数据*******************
              // send方法接收一个参数,即要作为请求主体发送的数据. 如果不需要通过请求主体发送数据, 则必须传入null. 因为这个参数对有些浏览器是必须的
              xhr.send(null);
              // 因为使用get方式提交，所以可以使用null参调用

// 如果要设置请求头部信息,必须在调用open()方法之后且调用send()方法之前调用setRequestHeader()
```
- readyStatus的五个阶段
    - 0：未初始化。尚未调用open()方法
    - 1：启动。已经调用open()方法，尚未调用send()方法
    - 2：发送。已经调用send()方法，尚未接收到响应
    - 3：接收。已经接收部分响应数据。
    - 4：完成。已经接收到全部响应数据，而且已经可以在客户端使用了。【一般只需检查这个阶段】
- 获得的数据在responseText或responseXML属性中, 后者需要XML解析

## ajax跨域

三种跨域方法：
一、一般使用封装好的jsonp

jsonp的核心则是动态添加`<script>`标签来调用服务器提供的js脚本。
普通方法: 给html标签添加脚本属性
```
function addScriptTag(src) {
  // 创建script元素标签，设置其属性
  var script = document.createElement('script');
  script.setAttribute("type","text/javascript");
  // 给script标签，设置标签属性
  script.src = src;
  // 把script标签添加成为body的子标签
  document.body.appendChild(script);
}
// 提供jsonp服务的url地址，并调用foo函数
window.onload = function () {
  addScriptTag('http://example.com/ip?callback=foo');
}

function foo(data) {
  console.log('Your public IP address is: ' + data.ip);
};
```

jqurey方法：
```
 $.ajax({
    url: "url",
    type: "GET",
    dataType: "jsonp",  //指定服务器返回的数据类型
    jsonp: "cb",   //指定参数名称
    jsonpCallback: "showData",  //指定回调函数名称
    success: function (data) {
        console.log("调用success");
    }
});
```

二、CORS是跨源资源分享（在后端设置头部支持，允许跨域）
- Access-Control-Allow-Origin:*

三、设置代理请求（不会）

# vue使用Ajax

本身不支持发送AJAX请求，需要使用vue-resource、axios等插件实现

## axios使用Ajax

注意：axios不支持跨域

参考github上的官方文档，[options]是可以使用的方法
```
axios([options])  
axios.get(url[,options]);
    传参方式：
        1.通过url传参
        2.通过params选项传参
axios.post(url,data,[options]);
    axios默认发送数据时，数据格式是Request Payload，并非我们常用的Form Data格式，
    所以参数必须要以键值对形式传递，不能以json形式传参
    传参方式：
        1.自己拼接为键值对
        2.使用transformRequest，在请求发送前将请求数据进行转换
        3.如果使用模块化开发，可以使用qs模块进行转换

axios本身并不支持发送跨域的请求，没有提供相应的API，作者也暂没计划在axios添加支持发送跨域请求，所以只能使用第三方库
```
get请求示例
```
写法一（只能用这个，参考上面的传参方式）
axios.get('/user?ID=12345')
  .then(function (response) {// 成功
    console.log(response);
  })
  .catch(function (error) {// 失败
    console.log(error);
  });

写法二
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

## 使用vue-resource发送跨域请求

基本用法
```
使用this.$http发送请求  
    this.$http.get(url, [options])
    this.$http.head(url, [options])
    this.$http.delete(url, [options])
    this.$http.jsonp(url, [options])
    this.$http.post(url, [body], [options])
    this.$http.put(url, [body], [options])
    this.$http.patch(url, [body], [options])
```

```
this.$http.jsonp('https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su',{
				params:{
					wd:this.keyword
				},
				jsonp:'cb'
			}).then(resp => {
		this.myData=resp.data.s;
	});
```