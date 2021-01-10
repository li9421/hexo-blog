---
title: Express
date: 2021-01-11 00:17:06
categories: 
  - 后端
tags: 
  - Node
  - express
toc: true
---


作者：Tj

原生的http在某些方面表现不足以应对我们的开发需求，所以就需要使用框架来加快我们的开发效率，框架的目的就是提高效率，让我们的代码高度统一。

在node中有很多web开发框架。主要学习express
<!-- more -->

 `http://expressjs,com`其中主要封装的是http。

 ```javascript
  // 1.安装
  // 2.引包
  var express = require('express')
  var path = requre('path')
  // 3.创建服务器应用程序
  // 		也就是原来的http.createServer();
  var app = express();
  
  // 公开指定目录
  // 只要通过这样做了，就可以通过/public/xx的方式来访问public目录中的所有资源
  // 在Express中开放资源就是一个Api的事
  app.use('/public',express.static(path.join(__dirname,'/public')));
  
  // 模板引擎在Express中开放模板也是一个API的事
  
  // 当服务器收到get请求 / 时候，执行回调函数
  app.get('./',function(req,res) {
      res.send('hello,express')
  })
  
  // 相当于server.listen
  app.listen(300,funcction(){
             console.log('app is running at port 3000');
  })学习Expres
  ```

## 学习Express

### 起步

安装：[图片丢失]

```javascript
cnpm install express
```

hello world: 【图片丢了】

```javascript
// 引入 express
var express = require('express')

// 1.创建 app
var app = express();

// 2.
app.get('/',function(req,res) {
    // 1.
    // res.write('Hello');
    // res,weite('World');
    // res.end()
    
    // 2
    // res.end('hello wrold')
    
    // 3
    res.send('hello world')
})

app.listen(3000,function() {
    console.log('express app is runing...')
})
```

##### 基本路由

路由：

+ 请求方法
+ 请求路径
+ 请求处理函数

get:

```javascript
// 当你以get方法请求/的时候，执行对应的处理函数
app.get('/',function(req,res){
	res.send('hello world')
})      
```

post:

```javascript
// 当你以post方法请求/的时候，执行对应的处理函数
app.post('/',function(req,res) {
    res,send('hello world')
})
```

#### Expresss静态服务api

```javascript
// app.use不仅仅用来处理静态资源的，还可以做很多工作（body-parser的配置等等）
app.use(express.static('public'));

app.use(express.static('files'));

app.use('/static',express.static('public'));
```

```javascript
// 引入express 
var express = require('express')

// 创建app
var app = express();

// 开放静态资源
// 1.当以//public/开头的时候，去./public/目录中找对应的资源
// 访问： http://127.0.0.1:3000/public/login.html
app.use('/public/',express.static(patn.join('__dirname','/public/')))

// 2.当省略第一个参数的时候，可以通过省略/public的方式来访问
// 访问： http:/127.0.0.1:3000/login.html
// app.use(express.static('./public/'));

// 3.访问： http://127.0.0.1:3000/a/logi.html
// a 相当于public的别名
// app.use('/a/',express.static('./public/'));

// 
app.get('/',function(req,res) {
    res.end('hello world');
});

app.listen(3000,function(){
    console.log('express app is running')
})
```

#### 在Express中配置使用`art-template`模板引擎

+ [art-template官方文档](https://aui.github.io/art-template/)
+ 在node中，有很多第三方模板引擎都可以使用有`art-template`
  + 还有ejs，jade（pug），handlebars，nunjucks

安装：

```shell
npm install --save art-template
npm install --save express-art-template

// 两个一起安装
npm i --save art-template express-aet-template
```

配置：

```javascript
app.engine('html',require('express-art-template'));
```

使用：

```javascript
app.get('/',function(req,res) {
    // express默认会去views目录找index.html
    res.render('index.html',{
        title:'hello world'
    });
})
```

如果希望修改默认的 views 视图渲染存储目,可以”

```javascript
// 第一个参数千万不要写错
app.set('views',目录路径)
// 一般不要去改 就使用views 是一种约定
```

##### 在Expresss中获取表单请求路径

###### 获取get请求数据：

Express内置了一个API，可以直接通过`req.query`来获取数据

```javascript
// 通过req.query 方法获取用户请求输入的数据
// req.query只能拿到get请求的数据
var comment = req.query;
```

###### 获取post请求数据：

在Express中没有内置获取表单的post请求体的api，这里我们需要使用一个第三方包的`body0parser`来获取数据

安装：

```shell
npm install --save body-parser
```

配置：

// 配置解析表单 POST 请求体插件（注意 一定要在app.use(router)之前）

```javascript
const express = require('express')
// 引包
const bodyParser = require('body-parser')

const app = express()

// 配置body-parser
// 只要加入这个配置，则在req的请求对象上会多出来属性： body
// 也就是说可以直接通过 req.body 来后去表单post请求数据
// parser application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ exrended: false}))

// parse application/json
app.use(bodyParser.json())
```

使用：

```javascript
app.use(function(req,res) {
    res.setHeader('Comtent-type','text/plain')
    res.write('you posted:\n')
    // 可以通过req.body来获取表单请求数据
  res.end(JSON.stringify(req.body, null, 2))
})
```

### 在Express中配置使用`express-session`插件操作

> 参考文档：<https://github.com/expressjs/session>

安装：

```shell
npm install express-session
```

配置：

```javascript
// 该插件会为req请求对象添加一个成员： req.session默认是一个对象
// 这是最简单的配置方式
// Session是基于 cookie 实现的
app.use(session({
    // 配置加密字符串，他会在原有的基础上和字符串拼接起来去加密
    // 目的是为了增加安全性，防止客户端恶意伪造
    secret: 'keyboard cat',
    resave: false,
    saveUninitialized: true, // 无论是否适用Session，都默认直接分配一把钥匙
    cookie: { secure: true}
}))
```

使用：

```javascript
// 读
// 添加 Session 数据
// session就是一个对象
req.session.foo = 'bar';

// 写
// 获取 session 数据
res.session. foo

// 删
req.session.foo = null;
delete req.session.foo
```

提示：

默认Session 数据是内存储数据，服务器一旦重启，真正的生产环境会把Session进行持久化存储。

## 利用Express实现ADUS项目

### 模块化思想

模块化如何划分：

+ 模块职责要单一

JavaScript模块化：

+ Node中的 CommonsJS
+ 浏览器中的：
  + AMD require.js
  + CMD  sea.js
+ es6中增加了官方支持

### 起步

+ 初始化
+ 模板处理

### 路由设计

| 请求方法 | 请求路径         | get参数 |        post参数         | 备注             |
| -------- | ---------------- | ------- | :---------------------: | ---------------- |
| GET      | /students        |         |                         | 渲染首页         |
| GET      | /students/new    |         |                         | 渲染添加学生页面 |
| POST     | /students/new    |         | name,age,gender,hobbies | 处理添加学生请求 |
| GET      | /students/edit   | id      |                         | 渲染编辑页面     |
| POST     | /students/edit   |         |                         | 处理编辑请求     |
| GET      | /students/dalete | id      |                         | 处理删除请求     |

### 提取路由模块

router.js

```javascript
/**
* router.js路由模块
职责：
	处理路由
	根据不同的请求方法+请求路径涉资具体的请求函数
	模块职责要单一，我们划分模块的目的就是增强代码的可维护性，提高开发效率
*/

var fs =requires('fs');

// Express 专门 提供了一种更好的方式
// 专门提供路由的
var express = require('express');
// 1.创建一个路由容器的
var router = express.Router();
// 2. 把路由都挂载到路由容器中

router.get('/students',function(req,res){
  // res.send('hello world');
  // readFile的第二个参数是可选的，传入utf-8就是告诉他把读取到的文件直接按照utf8编码，直接转成我们认识的字符串
  // 除了这样来转换，也可以通过data。toString（）来转换
    fs.readFile('./db.json','utf8',function(err,data) {
        if(err) {
            return res.status(500).send('Server error.')
        }
        // 读取到的文件数据是string类型的数据
        // console.log(data);
        // 从文件按中读取到的数据据一定是字符串，所以一定要手动转化成对象
        var students = JSON.parse(data).students;
        res.render('index.html',{
            // 读取文件数据
            students: students
        })
    })
   })

router.get('/students/new',function(req,res) {
    res.render('new.html')
});

router.get('\students/edit',function(req,res) {
    
})

router.post('/students/edit',function(req,res) {
    
})

// 3.把router导出
module.exports = router;
```

app.js:

```javascript
var router = require('./router')

// router(app);
// 把路由容器挂载到app服务中
// 挂载路由
app.use(router);
```

### 设计操作数据的API文件模块

es6中的find和findindex：

find接收一个方法作为参数，方法内部返回一个条件

find会遍历所有的元素，执行你给定的带有条件返回值的函数

符合该条件的元素会作为find方法的返回值

如果遍历结束还没有符合该条件的元素，则返回undefined(图片丢了)

```javascript
/**
student.js 
数据操作模块
职责： 操作文件中的数据，只处理数据，不关心也业务
*/

const fs = require('fs');
/**
获取所有学生列表
return【】
*/
exports.find = function() {
    
}


/**
* 获取添加保存学生
*/
expors.savr= function() {
    
}

/**
* 更新学生
*/
exports.undate = function() {
    
}

/**
* 删除学生
*/
exports.delete = function() {
    
}
```

### 步骤

+ 处理模板
+ 配置静态开放资源
+ 配置模板引擎
+ 简单的路由，/studens渲染静态页出来
+ 路由设计
+ 提取路由模块
+ 由于接下来一系列业务操作都需要处理文件数据，所有我们需要封装Student.js
+ 先学好students.js文件结构
  + 查询所有学生列别的API
  + findById
  + save
  + updateByid
  + daleteid
+ 实现具体功能
  + 通过路由收到请求
  + 接受请求中的参数（get，post）
    + req.query
    + req.body
  + 调用数据操作API处理数据
  + 根据操作结果给客户端发送请求
+ 业务功能顺序
  + 列表
  + 添加
  + 编辑
  + 删除

### 子模版和模板的继承（模板引擎高级语法）【include，extend，block】

注意： 

模板页

```html
<!DOCTYPE html>
<html lang="zh">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<meta http-equiv="X-UA-Compatible" content="ie=edge">
	<title>模板页</title>
	<link rel="stylesheet" href="/node_modules/bootstrap/dist/css/bootstrap.css"/>
	{{ block 'head' }}{{ /block }}
</head>
<body>
	<!-- 通过include导入公共部分 -->
	{{include './header.html'}}
	
	<!-- 留一个位置 让别的内容去填充 -->
	{{ block  'content' }}
		<h1>默认内容</h1>
	{{ /block }}
	
	<!-- 通过include导入公共部分 -->
	{{include './footer.html'}}
	
	<!-- 公共样式 -->
	<script src="/node_modules/jquery/dist/jquery.js" ></script>
	<script src="/node_modules/bootstrap/dist/js/bootstrap.js" ></script>
	{{ block 'script' }}{{ /block }}
</body>
</html>
```

模板的继承：

header页面：

```html
<div id="">
	<h1>公共的头部</h1>
</div>
```

footer页面：

```html
<div id="">
	<h1>公共的底部</h1>
</div>
```

模板页的使用：

```html
<!-- 继承(extend:延伸，扩展)模板也layout.html -->
<!-- 把layout.html页面的内容都拿进来作为index.html页面的内容 -->
{{extend './layout.html'}}

<!-- 向模板页面填充新的数据 -->
<!-- 填充后就会替换掉layout页面content中的数据 -->
<!-- style样式方面的内容 -->
{{ block 'head' }}
	<style type="text/css">
		body{
			background-color: skyblue;
		}
	</style>
{{ /block }}
{{ block 'content' }}
	<div id="">
		<h1>Index页面的内容</h1>
	</div>
{{ /block }}
<!-- js部分的内容 -->
{{ block 'script' }}
	<script type="text/javascript">
		
	</script>
{{ /block }}
```

最终结果：【图片丢了】

# Express中间件

## 中间件的概念

> 参考文档：http://expressjs.com/en/guide/using-middleware.html

【图片丢失】

中间件： 把很复杂的事情分割成单个，然后依次有条理的执行。就是一个中间处理环节，有输入，有输出。

说的通俗易懂点儿，中间件就是一个（从请求到响应调用的方法）方法。

把数据从请求到响应分步骤来处理，每一个步骤就是一个中间处理环节。

```javascript
var http = require('http');
var url = require('url');

var cookie = require('./expressPtoject/cookie');
var query = require('./expressPtoject/query');
var postBody = require('./expressPtoject/post-body');

var server = http.createServer(function(){
    // 解析请求地址中的get参数
    // var obj = url.parse(req.url,ture);
    // req.query = obj.query;
    query(req,res); // 中间件
    
    // 解析请求地中中的post参数
    req.body = {
        foo:'bar'
    }
});

if (req.url === 'xxx') {
    // 处理请求
    。。。
 }

server.listen(3000,function(){
    console.log('3000 running');
});
```

同一个请求对象所经过的中间件都是同一个请求对象和响应对象。

```javascript
const express = require('express');
const app = exprss();
app.get('/abc',function(req,res,next) {
    // 同一个请求的req和res 是一样的，
    // 可以前面存储下面调用
    console.log('/abc');
    // req.foo = 'bar';
    req.body = {
        name: 'xiaoxiao',
        age: 18
    }
    next();
});
app.get('/abc',function(req,res,next){
    // console.log(req,foo)
    console.log(req.body);
    console.log('/abc');
});
app.listen(3000,function() {
	console.log('app is running at port 3000.')
}
```

【图片丢失】

## 中间件的分类：

### 应用程序级别的中间件

万能匹配（不关心任何请求路径和请求方法的中间件）:

```javascript
app.use(function(req,res,next){
    console.log('Time',date.now());
    next()
});
```

关心请求路径和请求方法的中间件：

```javascript
app.use('/a',function(req,res,next){
    console.log('Time',Date.now());
    next();
});
```

### 路由级别的中间件

严格匹配请求路径和请求方法的中间件

get：

```javascript
app.get('/',function(req,res) {
   	res.send('get'); 
});
```

post：

```javascript
app.post('/a',function(req,res) {
    res.send('post')
})
```

put: 

```javascript
app.put('/user',function(req,res){
 	res.send('put')   
})
```

dalete: 

```javascript
app.delete('/dalete',function(req,res){
 	res.send('delete')   
})
```

### 总

```javascript
const express = require('express')
const app = express();

// 中间件 ： 处理请求，本质就是个函数
// 在express  对中间件有几种分类

// 1.不关心任何请求路径和请求方法的中间件
// 也就是说任何请求都会进入这个中间件
// 中间件本身是一个方法，该方法接受三个参数
// Request 请求对象
// response 响应对象
// next 下一个中间件
// // 全局匹配中间件
// app.use(function(req,res,next){
// console.log('1')''
// })
//  当一个请求进入中间件后；
//	如果需要请求另一个方法则需要使用next()方法
//  next（）；
// next 方法是一个方法，用来调用下一个中间件
// 注意 next（）方法调用下一个方法的时候，也会匹配（不是调用紧挨着的哪一个）
// app.use(function(req,res,next){consolg.log('2');});

// 2.关心请求路径的中间件
// 以/xxx开头的中间件
// app.use('/a',function(req,res,next) {console.log(req.url);})

// 3.严格匹配请求路径和请求方法的中间件
app.get('/',function(){
    console.log('/');
});

app.post('/a',function() {
    console.log('/a')
});

app.listen(3000,function(){
    console.log('app is running au port 3000')
});
```

## 错误处理中间件

```javascript
app.use(function(err,req,res,next) {
    console.error(err,stack);
    res.statu(500).send('Someting broke');
});

```

配置使用404 中间件：

```javascript
app.use(function(req，res) {
    res.render('404.html')
});
```

配置全局错误处理中间件：

```javascript
app.get('/a',function(eq,res,next) {
    fs.readFile('.a/bc',function(err) {
        if (err) {
            // 当调用next()传参后，则之间进入全局错误处理中间件方法中
            // 当发生全局错误的时候，我们可以调用next传递错误对象
            // 然后被全局错误处理中间件匹配到并进行处理
            next(err);
        }
    })
});
// 全局错误处理中间件
app.use(function(err,req,res,next) [
    res.status(500).json({
        err_code: 500,
        message: err.message
    });
]);
```



## 内置中间件

+ express.static（提供静态文件）
  + http://expressjs.com/en/starter/static-files.html#serving-static-files-in-express

## 第三方中间件

> 参考文档：http://expressjs.com/en/resources/middleware.html

+ body-parser
+ compression
+ cookie-parser
+ mogran
+ response-time
+ server-static
+ session

# 更换头像功能

## 引入插件 `multer`

> 参考链接  https://juejin.im/entry/6844903763937853453
>
> multer 文档：https://github.com/expressjs/multer

### 按照

```shell
npm install --save multer
```

### 引包+配置中间件

```javascript
var express = require('express')
var multer  = require('multer')
// var upload = multer({ dest: 'uploads/' }) // 设定上传文件存放位置 但是这个设置太简单不好用

// 这个设置 比较好用 
var storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, 'public/img') // 设置文件存放位置 相对程序启动目录位置
  },
  filename: function (req, file, cb) {
    cb(null, Date.now() + file.originalname) // 设定文件拓展名 一定要设置才能使用这张图片
  }
})

var upload = multer({ storage: storage })



var app = express()

// 上传单张图片，并指定上传时input的name为avatar
app.post('/profile', upload.single('avatar'), function (req, res, next) {
  // req.file 对象类型，包含上传文件的基本信息
  // req.body 将具有文本域数据，如果存在的话
})

// 上传12张图片，指定上传时input的name为photos
app.post('/photos/upload', upload.array('photos', 12), function (req, res, next) {
  // req.files 数组类型，包含多个file
  // req.body 将具有文本域数据，如果存在的话
})

var cpUpload = upload.fields([{ name: 'avatar', maxCount: 1 }, { name: 'gallery', maxCount: 8 }])
app.post('/cool-profile', cpUpload, function (req, res, next) {
  // req.files 是一个对象 (String -> Array) 键是文件名，值是文件数组
  //
  // 例如：
  //  req.files['avatar'][0] -> File
  //  req.files['gallery'] -> Array
  //
  // req.body 将具有文本域数据，如果存在的话
})
```

案例

```javascript
// 接受客户上传头像
router.post('/settings/profile/pic_upload', upload.single('avatar'), function(req, res, next) {
    console.log(req.file);

    // req.session.user.avatar = '/public/img/' + req.file.filename
    User.findOneAndUpdate(req.session.user._id, {
            avatar: '/public/img/' + req.file.filename
        // 将文件名替换掉数据库存储的默认文件名或者上一个文件名
        }, function(err, data) {
            if (err) {
                return next(err)
            }
            User.findOne({
                email: data.email
            }, function(err, data) {
                if (err) {
                    return next(err)
                }
                req.session.user = data
                res.status(200).json({
                    err_code: 0,
                    message: 'ok',
                    imgurl: data.avatar // 将图片路径传给前端，用来改变田端的图片显示
                })
            })
        })
        // res.redirect('/settings/profile')

})
```

### 前端上传头像写法【重要】

form 表单直接提交写法

```html
<form action="/profile" method="post" enctype="multipart/form-data">
    <h2>单图上传</h2>
    <input type="file" name="avatar">
    <input type="submit" value="提交">
</form>

```

form 表单提交需要加`enctype="multipart/form-data"`，确保`Content-Type`是`multipart/form-data`

### JQuery 异步提交

```javascript
<input type="file" name="avatar" id="fileUploader">
<button onclick="uploadFile()">上传</button>

<script src="http://cdn.bootcss.com/jquery/3.1.0/jquery.min.js"></script>
<script>
    function uploadFile () {
        var formData = new FormData();
        var file = document.getElementById('fileUploader').files[0];
        // 这里指定name值，input指定的name不起作用
        formData.append('avatar', file);
        $.ajax({
            url        : '/profile',
            type       : 'post',
            data       : formData,
            // processData 默认true，会将data转化为string传输，这里必须设置为false
            processData: false,
            // contentType 默认'application/x-www-form-urlencoded; charset=UTF-8'
            // 这里不要设置为multipart/form-data，会丢失部分信息，设置为false，会自动识别contentType
            contentType: false,
            success    : function (res) {
                console.log(res);// 最好这里返回一个路径 直接替换掉 目标元素的src属性 
            },
        })
    }
</script>
```

