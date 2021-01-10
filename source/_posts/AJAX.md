---
title: AJAX
date: 2021-01-01 22:32:04
# cover:
categories: 
  - 前端
tags: 
  - AJAX
toc: true
---
# HTTP

HTTP (hypertext tranport protocol) 协议 【超文本传输协议】，协议详细规定了浏览器和万维网服务器之间互相通信的规则。 是约定，是规则

<!-- more -->
## 请求报文

重点是格式和参数

```
行 	POST /s?ie=utf-8 HTTP/1.1
头 	HOST: atguigu.com
	 Cookir: name=guigui
	 Content-type: application/x-ww-from-urlencoded
	 User-Agent: chrome 83
	 
空行
体 	username=admin&password=admin
```

## 响应报文

```
行 	HTTP/1.1 200 OK
头 	Content-type： text/html;charset=utf-8
	 Content-length: 2048
	 Content-encoding: gzip
	 
空行 
体 	<html>
		<head>
		</head>
		<body>
		</body>
	</html>	
```

# 响应代码

## http响应中常见的几种响应代码

1. 200响应代码

   + Status Code: 200表示响应成功，很常见

2. 300响应代码

   + Status Code: 301 表示客户端跳转(重定向）,永久性跳转，一般在servlet 中使用如下代码

     ```
     response.setStatus(301);
     response.setHeader("Location","fail.html")
     ```

3. 302响应代码

   + Status Code : 302  客户端跳转，临时跳转，比如 访问页面  `a.html`,提交数据后就会跳转到 `b.html`

     ```
     response.sendRedirect("/a");
     ```

4. 304 响应代码

   + Status  Code : 304 表示资源未被修改，当不是第一次访问一个静态页面或者图片的时候，就会得到这么一个提示，这是服务端提示浏览器，这个资源没有发生改变，你直接使用上一次下载的就行了，不需要重新下载。这样就节约了带宽，并且浏览器的加载速度也会更快。

5. 404 响应代码

   + Status Code ：404 表示访问的页面不存在，表示一个浏览器的错误，就是服务端没有提供这个服务，但是你却去访问。一般检查路径问题

6. 500 响应代码

   + Status Code : 500 表示服务端错误，一般检查servlet。

### 其他相关代码

+ 100 表示继续
+ 401 表示未授权
+ 402 表示需要付费(很少见)
+ 403 表示禁止
+ 405 表示方法不被允许
+ 406 表示无法请求 (很少见)
+ 408 表示请求超时 
+ 413 表示实体过大 (不知道什么意思不用深究)
+ 507 表示存储不足 

---



# Ajax

##  1. 原生AJAX

### 1.1 AJAX 简介

​	AJAX 全称为 **`Asynchronous JavaScript And XML`** 就是异步的JS 和 XML 。

​	通过 AJAX 可以在浏览器向服务器发送异步请求，最大的优势： **无刷新获取数据**。

​	AJAX 不是新的编程语言，而是一种将现有的标准组合在一起使用的新方式。

### 1.2 XML 简介

​	XML 可拓展标记语言。

​	XML 被设计用来传输和存储数据。

​	XML 和 HTML 相似，不同的是 HTML 中都是预定义标签，而 XML 中没有预定义标签，全都是自定义标签，用来表示一些数据。

```xml
比如说我有一个学生：
name:"孙悟空";age =18; gender="男"
用 XML 表示：
<student>
    <name>孙悟空</name>
    <age>18</age>
    <gender>男</gender>
</student>
但是现在已经被 JSON 取代了
用JSON 表示：
{"name": "孙悟空","age": 18,"gender"="男"}
```

### 1.3 AJAX 的特点

#### 	1.3.1 AJAX的优点

		1. 可以无需刷新页面而与服务器进行通信。

     		2. 允许你根据用户事件来更新部分页面内容

#### 1.3.2 AJAX 的缺点

1. 没有浏览历史，不能回退
2. 存在跨域问题(同源)
3. SEO 不友好

### 1.4 AJAX 的使用

#### 1.4.1 核心对象

​	`XMLHttpRequest`: AJAX 是所有操作都是通过该对象进行的。

#### 1.4.2 使用步骤

```js
// 1. 创建XMLHttpRequest 对象
var xhr = new XMLHttpRequest();
// 2. 设置请求信息
xhr.open(method.url); // method是求方法 ，get/post 当然还有别的
// 可以设置请求头，一般不设置
xhr.setRequestHeader('content-Type','application/x-www-form-urlencoded');
// 3. 发送请求
xhr.send(body) // get 请求 不传body 参数 ， 只有 post 请求使用
// 4. 接受响应
// xhr.responseXML 接受 xml 格式的响应数据
// xhr.responseText 接受 文本格式的响应数据

xhr.onreadustatechange = function() {
    if(xhr.readuState == 4 && sht.status == 200) {
      var text = xhr.responseText;
      console.log(text);
   }
}
```

#### 1.4.3 解决id缓存问题

​	问题： 在一些浏览器(IE) ,由于缓存机制的存在，ajax 只会发生的第一次请求，剩余多次请求不会再发送给浏览器，而是直接加载缓存中的数据。

​	解决方式： 浏览器的缓存是根据 url 地址来记录的，所以我们只需要修改 url 地址即可避免缓存问题

```js
function(){
  const xhr = new XMLHttpRequest();
   xhr.open('GET', 'http://127.0.0.1:8000/ie?t=' + Date.now());
    // 在url 后面加上参数为当前时间轴，就可以保证每次发送请求的链接都不一样
   xhr.send();
   xhr.onreadystatechange = function() {
       if (xhr.readyState === 4) {
           if (xhr.status >= 200 && xhr.status < 300) {
               result.innerHTML = xhr.response;
           }
       }
   }
}
```

#### 1.4.4 AJAX 请求状态

​	`xhr.readyState` 可以用来查看请求当前的状态

+ 0 表示 XMLHttpRequest 实例已经生成，但是 open()方法还没有调用。
+ 1 表示 send() 方法还没有调用，仍然可以使用 setRequestHeader(),设定 HTTP请求头信息
+ 2 表示send() 方法已经执行，并且头信息和状态码已经收到。
+ 3 表示正在 接受服务器传来的 body 部分数据。
+ 4 表示服务器数据已经完全接收，或者本次接收已经失败

## 2. jQuery 中的 AJAX

### 2.1 get 请求

```js
$get(url,[data],[callback],[type])
url: 请求的 URL 地址
data: 请求携带的参数。
callback: 载入成功时的回调函数。
type： 设置返回内容的格式， xml。html,script.json text,_default

实例
  $.get('http://127.0.0.1:8000/jquery-server', {
        a: 100, b: 200},
  function(data) {
       console.log(data);
  }, 'json')
```

### 2.2 post 请求

```js
$.post(url, [data], [callback], [type])
url:请求的 URL 地址。
data:请求携带的参数。
callback:载入成功时回调函数。
type:设置返回内容格式，xml, html, script, json, text, _default。

实例
 $.post('http://127.0.0.1:8000/jquery-server', {
    a: 100,b: 200
}, function(data) {
    console.log(data);
})
```

### 2.3 通过方法

```js
 $.ajax({
   // url
   url: 'http://127.0.0.1:8000/jquery-server',
   // 发送参数 
   data: {
       a: 100,
       b: 200
   },
   // 请求的类型
   type: 'GET',
   // 响应体结果
   dataType: 'json',
   // 成功的回调
   success: function(data) {
       console.log(data);
   },
   // 失败的回调
   error: function() {
       console.log('出错啦');
   },
   // 超时时间
   timeout: 2000,
   // 头信息 
   headers: {
       c: 300,
       d: 400,
   }
})
```

---

## 3. 跨域

### 3.1 同源策略

​	同源策略（Same-Origin Policy）最早是由 Netscape 网景公司提出的,浏览器的一种安全策略。

​	同源： 协议、域名、端口号 必须完全相同

​	违背同源策略就是跨域。

### 3.2 如何解决跨域

#### 3.2.1 JSONP

1. JSONP是什么

   JSONP(JSON with Padding),是一个非官方的跨域解决方案，纯粹凭借程序员的聪明才智开发出来的，只支持get 请求。

2. JSONP怎么工作

   在网页中有一些标签天生具有跨域能力，比如  img link iframe script.

   JSONP 就是利用 script 标签 的跨域能力 来发送请求。

3. JSONP 的使用

   1. 动态创建一个 script 标签

      var script = doucument.createElement("script");

   2. 设置 script 的 src ，设置回调函数

      script.src = "http://loaclhost:3000/testAjAx?callback=abc";

      function abc(data) {

      ​	alert(data.name);

      };

   3. 将 script 添加到 body 中

      document.body.appendChild(script);

   4. 服务器中的路由处理

      router.get("/testAJAX",function(req,res) {

      console.log('收到请求')

      var callback = req.query.callback;

      var obj = {

      name:"孙悟空"，

      age:18

      }

      res.send(callback+"("+JSON.stringify(obj)+")");

      })

4. jQuery 中的 JSONP 

```JS
$.getJSON('http://127.0.0.1:8000/jquery-jsonp-server?callback=?',function(data) {
    $('#xxx').html{
    `名称： ${data.name},</br>
	校区： ${data.city}`)})
```

#### 3.2.2 CORS

https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS [MDN的CORS]

1. CORS 是什么

   CORS(Cross-Origin Resource Sharing), 跨域资源共享。CORS 是官方的跨域解决方案，它的特点是不需要在客户端做任何操作，完全在服务器中进行处理，支持get 和 post 请求，跨域资源共享标准新增了一组 HTTP 首部字段， 允许服务器声明哪些源站通过浏览器有权限访问哪些资源

2. CORS 是怎么工作的？

   cors 是通过设置一个响应头来告诉浏览器，该请求允许跨域，浏览器收到该响应以后就会对响应放行

3. CORS 的使用

   主要是 服务器端的设置： 

   ```js
   router.get("/testAJAX",function(req,res){
   //通过 res 来设置响应头，来允许跨域请求 
   //res.set("Access-Control-Allow-Origin","http://127.0.0.1:3000"); 
   res.set("Access-Control-Allow-Origin","*");
   res.send("testAJAX 返回的响应");
   })
   ```

   

   