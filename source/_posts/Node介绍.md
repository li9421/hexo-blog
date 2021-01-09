---
title: Node.Js
date: 2021-01-09 21:22:45
categories: 
  - 后端
tags: 
  - Node
toc: true
---

## Node.js是什么

- Node.js是JavsScript 运行时
- 通俗易懂的讲，Node.js是JavaScript的运行平台
- Node.js既不是语言，也不是框架，它是一个平台
<!-- more -->
- 浏览器中的Javascript
  - EcmaScript
    - 基本语法
    - if
    - var
    - function
    - Object
    - Array
  - Bom
  - Dom
- Node.js中的javaScript
  - 没用Bom， Dom
  - Ecmascript
  - 在Node中这个javaSript执行环境为javaScript提供了一些服务器级别的API
    - 例如文件的读写
      - 网络服务的构建
        - 网络通信
    - Http服务
  - 构建于Chrome的V8引擎之上
    - 代码只是具有特定格式的字符串
    - 引擎可以认识它，帮你解析和执行
    - Googl Chrome的V8引擎是目前公认的解析执行javaScript代码最快的
    - Node.js的作者把Google Chrome中V8引擎移植出来，开发了一个独立的javaScript运行时环境
  - Node.js user an envent-driven,non-blocking I/O mode thatmakes it lightweight and efficent. + envent-driven 事件驱动 + non-block I/O mode 非阻塞I/O模型（异步） + ightweight and efficent. 轻量和高效
  - Node.js package ecosystem,npm,is the larget scosystem of open sourcr libraries in the world + npm 是世界上最大的开源生态系统 + 绝大多数的javaScript相关的包都存放在npm上，这样做的目的是为了让开发人员更方便的去下载使用 + npm install jquery

## 为什么要学习Node.js

- 企业需求
  - 具有服务端开发经验更佳
  - front-end
  - back-end
  - 全栈开发工程师
  - 基本的网站开发能立 + 服务端 + 前端 + 运维部署
  - 多人社区


## Node能做什么

```
+ web服务器后台
```

- 命令行工具
  - npm(node)
    - git(c语言)
    - hexo (node)
  - ...
- 对于前端工程师来讲，接触最多的就是它的命令行工具
  - 自己写的很少，主要是用别人第三方的
  - webpack
  - gulp
  - npm

# 起步

## 安装Node环境

```
+ 查看Node环境的版本号
+ 下载  <https://nodejs.org/en/>
```

- 安装
  - 傻瓜式安装，一路next
    - 安装过再次安装会升级
- 确认Node环境是否安装成功
  - 查看 node的版本号： `node --version`
  - 或者 `node -v`
- 配置环境变量

## 解析执行javaScript

```
1. 创建编写JavaScript脚本文件
2. 打开终端，定位脚本文件所在目录
3. 输入 `node 文件名` 执行对应的文件
```

注意文件名不要用`node.js` 来命名，也就是说除了`node`这个名字随便起，最好不要使用中文

## 文件的读写

文件读取：

```
// 浏览器中的Javascript是没用文件操作能力的
// 但是Node中的JanaScript具有文件操作能立
// fs是file-system的简写，就是文件系统的意思
// 在Node中如果想要进行文件的操作就必须要引入fs这个核心模块
// 在fs这个核心模块中，就提供了所有文件操作相关的API
// 例如 fs.readFile就是用来读取文件的

// 1.使用fs核心模块
var fs = require('fs');

// 2.读取文件
fs.readFile('./data/a.txt',function(err,data) {
    if(err) {
        console.log('文件读取失败')
    }else {
        console.log(data.toString())
    }
})
```

文件写入：

```
// 1.使用fs核心模块
var fs = require('fs')

// 2.将数据写入文件
fs.writeFile('./data/a.txt','我是文件写入的信息内容',function(err,data) {
    if(err){
        console.log('文件写入失败')
    }else {
        console.log(data.toString())
})
```

## http

服务器：

```
// 1.加载http核心模块

var http = require('http')

// 2.使用 http.createServer()创建一个web服务器
var server = http.cerateServer();

// 3. 服务器要做的事儿
/*
	提供服务： 对数据服务
	发请求
	接受请求
	处理请求
	反馈(发送响应)
	当客户端请求过来，就会自动触发服务器的request请求事件，然后执行第二个参数：回调处理函数
*/
sever.on('request',function(req.res) {
    console.log('收到客户端的请求了')
})

// 4.绑定端口号，启动服务
server.listen(3000,function() {
    console.log('running....')
})
```

# Node中的模块系统

使用Node编写应用程序主要就是在使用“

- EcmaScript语言
  - 和浏览器一样的，但是在Node中没有Bom，Dom
- 核心模块
  - 文件操作的fs
  - http服务操作的http
  - url路径操作模块
  - path路径操作模块
  - os操作系统信息
- 第三方模块
  - art-template
  - 必须通过npm下载才可以使用
- 自己写的模块
  - 自己创建的文件

## 什么是模块化

- 文件作用域(模块是独立的，在不同的文件使用必须要重新引用)【在node中没有全局作用域，它是文件模块作用域】
- 通信规则
  - 加载require
  - 导出exports (e不发音 读 艾克斯破事 )

## commonsJS模块规范

在Node中Javascript还有一个重要的概念，模块系统

- 模块作用域

- 使用require方法来加载模块

- 使用exports接口对象来导出模块中的成员

  ### 加载 require

  语法

  ```
  var 自定义变量名 = require('模块')
  ```

  作用:

  - 执行被加载模块的代码
  - 得到被加载模块中的 `exports`导出接口对象

  ### 导出`exports`

  - Node中是模块作用域，默认文件中所有的成员只在当前模块有效

  - 对于希望可以被其他模块访问的成员，我们需要把这些想要公开的成员都挂载到 `exports`对象中就可以了

    导出多个成员（必须在对象中）：

    ```
    exports.a = 123;
    exports.b = function() {
        console.log('bbb')
    };
    exports.c = {
        foo: "bar"
    };
    exports.d = 'hello';
    ```

```
导出单个成员（拿到的就是函数，字符串）：

 ```javascript
 module.exports = 'hello'
 ```

 以下情况会覆盖：

 ```javascript
 module.exports = 'hello';
 // 后者会覆盖前者
 module.exports = function add(x,y) {
     return x+y;
 }
 ```

 也可以通过以下方法来导出多个成员

 ```javascript
 module.exports = {
     foo = 'hello',
     add:function() {
         return x+y;
     }
 };
 ```
```

## 模块原理

exports 和 module.exports 的一个引用：

```
console.log(exports === module.exports);// true

exports.foo = 'bar';

// 等价于
module.exports.foo = 'bar'
当给expots重新赋值后，exports！=module.exports.
最终return的是module.exports,无论exports中的成员是什么都没有用
真正去使用的时候：
	导出单个成员： exports.xxx = xxx;
	导出多个成员： module.exports 或者 module.exports = {};
```

## 总结

```
// 引用服务
var http = require('http');
var fs = require('fs');
// 引用模板
var template = require('art-template');
// 创建服务
var server = http.createServer();
// 公共路径 
var wwwDir = 'D:/app/www';
server.on('request',function(req,res) {
    var url = req.url;
    // 读取文件
    fs.readFile('./template-apche.html',function(err,data) {
        if(err) {
            return res.end('404 NoT Found')
        }
        fs.readdir(wwwDir,function(err,files) {
            if (err) {
                return res.end('Can not find www Dir')
            }
            // 使用模板引擎解析替换data中的模板字符串
            // 去xmpTempletelist.html中编写模板语法
            var htmlStr = template.render(data.toString(),{
                title: 'D:/app/www/ 的索引',
                files: files
            
        });
            // 发送相应数据
            res.end(htmlStr)
    }
});
server.listen(3000,function() {
    console.log('running'....)
})        
1.jQuery中的each 和 原生JavaScript方法forEach的区别：
	提供源头：
    	原生js是es5提供的（不兼容IE8），
        jqurey的each是jQuery第三方库提供的（如果要使用需要用2以下的版本也就是1.版本），它的each方法主要用来遍历jQuery对象（伪数组），同时也可以做低版本forEach的替代品，jQuery的实例对象不可以用forEach方法，如果想要使用必须转为数组([].slice.call(Jquery实例对象)) 才能使用
2. 模块中导出多个成员和导出单个成员
3.301和302的区别： 
	301 永久重定向，浏览器会记住(只要不清缓存，下次就直接重定向，不问了)
    302 临时重定向 (下次还会再问)
4.exports和module.exports的区别：
	每个模块中都有一个module对象
    module对象中有一个exports对象
    我们可以把需要导出的对象都挂载到
    module.exports接口对象
	也就是`module.exports.xxx = xxx`的方师傅
    但是每次写太多了就很麻烦，所有 Node 为了简化代码，就在每个模块中都提供了一个成员叫`exports`
    `exports === module.exports`结果为 true，所以完全可以`exports.xxx = xxx`
    当一个模块需要导出单个成员的时候必须使用`module.exports = xxx`的方式， =使用`exports = xxx`不管用，因为每个模块最后return 的是module.exports,而exports只是module.exports的一个引用，所以`exports`即使重新赋值，也不会影响`module.exports`。
    有一种赋值方式比较特殊：`exports = module.exports`这个用来重新建立引用关系。
```

# require 的加载规则

- 核心模块
  - 模块名
- 第三方模块
  - 模块名
- 用户自己写的
  - 路径

## require的加载规则：

- 优先从缓存加载

- 判断模块标识符

  - 核心模块

  - 自己写的模块（路径形式的模块）

  - 第三方模块（node-modules）

    - 第三方模块的标识就是第三方模块的名称（不可能有第三方模块的核心模块的名字一致）

    - npm

      - 开发人员可以把写好的框架库发布到npm上
      - 使用者通过npm命令来下载

    - 使用方式：

      ```
      var 名称 = require('npm install 【下载包】的包名')
      ```

      - node_modules/express/package.json main
      - 如果package.json或者main不成立，则查找被选择项： index.js
      - 如果以上条件都不满足，则继续进入上一级目录中的 node_modules 按照上面的规则依次查找，直到当前文件所属磁盘根目录都找不到最后报错

```
// 如果非路径形式的标识
// 路径形式的标识：
	// ./ 当前目录 不可省略
	// ../ 上一级目录 不可省略
	// /xxx 也就是 D：/xxx 也叫磁盘根目录
	// 带有绝对路径几乎不用 （D:/a/foo.js）
// 首位表示的是当前文件模块所属磁盘根目录
// requir('./a');


// 核心模块
// 核心模块本质也是文件，核心模块文件已经被编译到了二进制文件中了，我们只需要按照名字来加载就可以了
require('fs');

// 第三方模块
// 凡是第三方模块都必须通过npm下载(npm i node_modules),使用的时候就可以通过require('包名')来加载使用
// 第三方包的名字不可能和核心模块的名字是一样的
// 既不是核心模块，也不是路径形式的模块
//  	先找到当前文件所属目录的node_modules
//		然后找node——modules/art-template目录
// 		mode_modules/art-template/package.json
// 		node_modules/art-template/package.json中的main属性
//		main属性记录了art-template的入口模块
//		然后就使用加载这个第三方包
//		实际最终加载的还是文件

// 		如果package.json不存在或者main 指定的入口模块不存在
// 		则node会自动找该目录下的index.js
//		也就是说index.js 是一个备选项，如果main没有指定，就加载index.js
// 
// 		如果条件都不满足则会进入上一级目录进行查找
// 注意： 一个项目只有一个node_modules,放在项目根目录中，子目录可以直接调用根目录的文件
var template = require('art-temolate')
```

## 模块标识符中的/和文件操作路径中的/

文件操作路径：

```
// 咱们所使用的所有文件操作的APi都是异步的
// 就像AJAX请求一样
// 读取文件
// 文件操作中 ./相当于当前模块所处的磁盘根目录
// ./index.txt  	相对于当前目录
// /index.txt  		相对于当前目录
// /index.txt		绝对路径，当前文件模块所处根目录
// d:/express/index.js   绝对路径
fs.readFile('./index.txt',function(err,data) {
    if(err){
        return consloe.log('读取失败')
    }
    console。log(data.toString());
})
```

模块操作路径

```
// 在模块加载中，相对路径中的./不能省略
// 这里省略了，也是磁盘根目录
require('./index')('hello')
```

# npm

- node package manage(node包管理器)
- 通过npm命令安装jQuery包（npm install --save jquery),在安装时加上 --save 会主动生成说明文件信息（将安装文件的信息添加到package.json里面 npm 5.版本后可以省略 --save 不管加不加都会自动保存）

### npm 网线

> [npmjs.com](http://npmjs.com/) 网站 是用来搜索npm包的

### npm命令行工具

npm是一个命令行工具，只要安装了node就可以安装npm

npm 也有版本概念，可以通过 `npm --version`来查看npm的版本

升级npm（自己升级自己）：

```
npm install --global npm
```

### 常用命令

- npm init(生成package.json说明书文件）
  - npm init -y(可以跳过向导，快速生成)
- npm install
  - 一次性把dependencies 选项中的依赖项全部下载
  - 简写 （npm i）
- npm install 包名
  - 只下载
  - 简写 （npm i 包名）
- npm install --save
  - 下载并保存依赖项 （package,json文件中的dependencies）
  - 简写 （npm i 包名）
- npm uninstall 包名
  - 只删除，如果有依赖项会保存
  - 简写 （npm un 包名）
- npm help
  - 查看使用帮助
- npm 命令 -- help
  - 查看具体命令的使用帮助 （npm uninstall--help）

### 解决npm被墙问题

npm存储包文件的服务器在国外，有时候会被墙，速度很慢，所以需要解决这个问题。

> https://developer.aliyun.com/mirror/NPM?from=tnpm 淘宝开发团队把npm 在国内做了一个镜像 (也就是一个备份)。

安装淘宝的cnpm

```
npm install -g cnpm -- registry=https://registry.npm.taobao.org
#在任意目录执行都可以
# --global表示安装到全局，而非当前目录
# --global不可以省略，否则不管用
npm install --global cnpm
```

安装包的时候把以前的`npm`替换成`cnpm`。

```
# 走国外的npm 服务器下载jQuery包，速度比较慢
npm install jQuery；

# 使用cnpm 就会通过淘宝的服务器来下载jQuery
cnpm install jQuery
```

如果不想安装 cnpm 又想使用淘宝的服务器来下载：

```
npm install jquery --registry=https://npm.taobao.org;
```

但是每次手动加参数就很麻烦（主要记不住），所以我们可以把这个选项加入到配置文件中：

```
npm config set registry https://npm.taobao.org;

# 查看npm配置信息
npm config list;
```

只要经过上面的配置命令，则以后所有的`npn install`都会通过淘宝的服务器来下载

# package.json

每个项目都要有一个`package.json`文件（包描述文件，就像产品是说明书一样）

这个文件可以通过 `npm init`自动初始化出来

```
PS D:\code\node> npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help init` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (node)
version: (1.0.0)
description: 这是一个测试项目
entry point: (index.js)
test command:
git repository:
keywords:
author:
license: (ISC)
About to write to D:\code\node\package.json:

{
  "name": "node",
  "version": "1.0.0",
  "description": "这是一个测试项目",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}

Is this OK? (yes) yes
```

对于目前来讲，最有用的是`dependencies`选项，可以用来帮助我们保存第三方包的依赖信息。

如果`node_modules`删除了也不用但是，只需要在控制面板中`npm install`就会自动把`package.json`中的`dependencies`中所有的依赖项全部下载下来。

- 建议每个项目的根目录下都有一个`package.json`文件
- 建议执行`npm install 包名`的时候都加上`--save`选项，目的是用来保存依赖信息

## package.json和package-lock.json

npm 5 以前是不会有`package-lock,json`这个文件

npm 5 以后才加入这个文件

当你安装包的时候，npm都会生成或者更新`package-lock.json`这个文件

- npm5以后的版本都不要加 `--save`参数，它会自动保存依赖信息

- 当你安装包的时候，自动创建或者更新`package.json`文件

- ```
  package-lock.json
  ```

  这个文件会包含

  ```
  node_modules
  ```

  中所有包的信息（版本，下载地址。。。）

  - 这样的话重新`npm install`的时候速度就可以提升

- 从文件来看，有一个

  ```
  lock
  ```

  称之为锁

  - 这个`lock`是用来锁版本的
  - 如果项目依赖了`1.1.1`版本
  - 如果你重新install 其实会下载 最新版本，而不是`1.1.1`
  - `package-lock.json`的另一个作用就是用来锁定版本号，防止自动升级

## path路径操作模块

> 参考文档：https://nodejs.org/docs/latest-v13.x/api/path.html

- path.basename: 获取路径的文件名，默认包含拓展名
- path.dirname: 获取路径中的目录部分
- path:extname: 获取一个路径中的拓展名部分
- patn.parse: 把路径转为对象
  - root:根路径
  - dir：目录
  - base： 包含拓展名的文件名
  - name： 不包含后缀名的文件名
- path.join: 拼接路径
- path.isAbsolute: 判断一个路径是否为绝对路径（图片丢失）

# Node中的其他成员(dirname,filename)

在每个模块中，除了`require`,`exports`等模块相关的API之外，还有两个特殊的成员：

- `__dirname`,是一个成员可以用来**动态**获取当前文件模块所属目录的绝对路径
- `__filename`,可以用来**动态**获取当前文件的绝对路径(包含文件名)
- `__dirname和__filename`是不受node命令所属路径影响的 [图片丢失]

在文件操作中，使用相对路径是不可靠的，因为node中文件操作的路径被设计为相对于执行node 命令所处的路径

所以为了解决这个问题，只需要把相对路径改为绝对路径(绝对路径不受任何影响)就可以了

就可以使用`__dirname 或者__filename`来帮助我们解决这个问题

在路径拼接的过程中，为了避免手动拼接带来的一些低级错 推荐使用 path.join()`来辅助拼接

```
var fs = require('fs');
var path =require('path');

//  console.log(__dirname + 'a.txt');
// path.join方法会将文件操作中的相对路径都统一的转为动态的绝对路径
fs.readFile(path.join(__dirname + '/a.txt'),'utf-8',function(err,data) {
    if (err){
        throw err
    }
    console.log(data);
});
```

> 补充： 模块中的路径标识和这里的路径没有关系，不受影响(就是相对于文件模块)

> #### **注意：**
>
> **模块中的路径标识和文件操作中的相对路径标识不一致**
>
> **模块中的路径标识就是相对于当前文件模块，补受node命令所处路劲影响**

# 修改完代码自动重启

我们在这里可以使用一个第三方命令行工具：

`nodemon`来帮助我们解决频繁修改代码重启服务器的问题。

`nodemon`是一个基于Node.js开发的第三方命令行工具，我们使用的时候需要独立安装；

```
# 在任意目录执行该命令都可以
# 也就是说，所有需要 --global 安装的包都可以在任意目录执行
npm install ---global nodemon
npm install -g nodemon

# 如果安装不成功的话 ，可以使用cnpm 安装
cnpm install -g nodemon
```

安装完毕之后使用：

```
node app.js

# 使用nodemon
nodemon app.js
```

只要是通过`nodemon`启动服务，则他会监视你的文件变化，当文件发生变化的时候，会自动帮你重启服务器。