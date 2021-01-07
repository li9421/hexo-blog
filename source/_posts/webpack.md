---
title: webpack
date: 2021-01-07 23:29:34
categories: 
  - 前端
tags: 
  - webpack 
  - 模块化
toc: true
---

## 认识webpack

[webpack官网](https://webpack.js.org/) (纯英文的可能看不懂)

[webpack中文网](https://www.webpackjs.com/) 

+ 什么是webpack

  从本质上来讲，webpack是一个现代的JavaScript应用的静态**模块打包**工具

  <!-- more -->

+ 主要两点 

  + 模块
  + 打包

### 前端模块化 ：

+ 前面已经提到过为什么需要模块化
+ 而且也提到了目前前端模块化的一些方案： AMD 、CMD、CommonJS、ES6 
+ 在ES6之前，我们要想进行模块化开发，就必须借助于其他的工具，让我们可以进行模块化开发
+ 并且在通过模块化开发完成项目后还需要处理模块间的各种依赖，并且将其进行整合打包。
+ 而webpack其中一个核心就是让我们可能进行模块化开发，并且会帮助我们处理模块间的依赖关系。
+ 而且不仅仅是JavaScript文件，我们的CSS、图片、JSON文件等等在 webpack中都可以被当作模块来使用(在后续我们会看到)。
+ 这就是webpack中的模块化的概念

### 打包:

打包如何理解呢？

+ 理解了webpack可以帮助我们进行模块化，并且处理模块间的各种复杂关系的后，打包的概念就非常好理解了。
+ 就是将webpack中的各种资源合并成一个或多个包（Bundle）
+ 并且在打包的过程中，还可以对资源进行处理，比如压缩图片，将scss/less等转成css，将ES6语法转成ES5语法，将typescrip转成JavaScript等等操作
+ 但是打包的操作似乎grunt/gulp也可以帮助我们完成，它们有什么不同呢



## webpack的安装

+ 必须先安装Node.JS
+ npm 全局安装 webpack

```shell
npm install webpack@3.6.0 -g ## 这里指定3.6.0版本因为vue cli2依赖该版本
```

+ 局部安装webpack 
  + `--save-dev` 指开发时依赖，项目打包后不需要继续使用的。

```shell
cd 对应目录
npm install webpack@3.6.0 --save-dev 
```

+ 为什么全局安装后还需要局部安装？
  + 在终端直接执行webpack命令，使用的全局安装的webpack
  + 当在package.json中定义了scripts 时，其中包含webpack 命令，那么使用的是局部webpack
  + 为避免全局与局部配置版本不同等，建议使用当前局部安装webpack打包当前的代码资源



## webpack的起步

准备工作：

+ 创建如下文件和文件夹
+ 文件和文件夹解析：
  + `dist` 文件夹： 用于存放打包后的文件
  + `src`  文件夹： 用于存放我们写的源文件
    + `main.js` : 项目的入口文件。具体内容查看详情
  + `index.html `: 浏览器打开展示的首页html
  + `package.json `： 通过`npm init` 生成的，npm包管理的文件

### JS文件的打包

现在的js文件中使用了模块化的方式进行打包，但是不可以直接使用

+ 因为如果直接在`index.html`引入这些js文件，浏览器并不能识别其中的模块化代码
+ 另外，在真实的项目中会有很多这样的js文件，我们一个一个引用非常麻烦，并且后期非常不方便对他们进行管理。

应该使用 webpack 工具 对多个js文件进行打包

+ 我们知道，webpack就是一个模块化的打包工具，所以它支持我们代码中写模块化，可以对模块化的代码进行处理。(具体原理看后面)
+ 另外，如果在处理完所有模块之间的关系后，将多个js文件打包到一个js文件中引入就方便很多了

webpack 打包指令

```shell
webpack src/main.js dist/bundle.js
```

打包后会在dist 文件下 生成一个 `bundle.js`文件

+ 文件很复杂 基本可以不看
+ `bundle.js`文件是webpack 处理了项目直接文件依赖后生成的一个js文件，我们只需要将这个js文件在`index.html`中引入即可



## webpack的配置

```shell
webpack ./src/main.js ./dist/bundle.js
```

###  入口和出口

我们考虑到：如果每次使用webpack的命令都需要写上入口和出口作为参数，就非常麻烦，有没有一个中方法可以将这个两个函数参数写道配置中，在运行的时候直接读取

#### webpack配置方法

+ 创建一个`webpack.json`文件

```json
const path - requre('path') // 引入node 默认模块

moudule.exprots = {
    // 入口： 可以是字符串/数组/对象，这里文明入口只有一个，所有写一个字符串及可
    entry: './src/main.js',
    // 出口，通常是一个对象，里面至少包含两个重要属性，path 和 filename
    output:{
        path: path.resolve(__dirname,'dist'),// 注意 path通常是一个绝对路径
        filename: 'bundle.js'
    }
}
```



### 局部安装webpack 

目前，我们使用的webpack是全局的webpack，如果我们想局部打包

+ 因为一个项目往往依赖特定的webpack版本，全局的版本可能和这个项目的webpack版本不一致，导致打包出现问题
+ 通常一个项目都有自己局部的webpack。

##### 第一步，项目中需要安装自己局部的webpack

+ 这里我们让局部安装webpack3.6.0

+ Vue CLi3中已经升级到了webpack4 但是它将配置文件隐藏起来了，所以查起来不是很方便。

  ```shell
  npm install webpack@3.6.0 --save-dev 
  ```

  

#### 第二步，通过`node_modules/.bin/webpack`启动webpack打包

##### `package.json`中定义启动

由于上面启动方式太麻烦，我们可以在`package.json`的`scripts`中定义自己的执行脚本

`package.json`中的`scripts`脚本在实行时，会按照一定的顺序寻找命令对应的位置

+  首先，会先寻找本地的`node_modules/.bin`路径中对应的命令

+ 如果没有找到，会去全局的环境变量中寻找

+ 如果执行我们的`build`指令

  ```shell
  npm run build
  ```



## loader的使用

### 什么是loader

loader是webpack中一个非常核心的概念

webpack用来做什么呢？

+ 在我们之前的实例中，我们主要用webpack 来处理我们写的js代码，并且 webpack 会自动处理js之间相关的依赖。
+ 但是，在开发中我们不仅仅有基本的js代码处理，我们也需要加载css，图片，也包括一些高级的将ES6转成ES5代码，将Typescript转成JavaScript，将scss、less转成css，将.jsx、.vue文件转成js文件等等。
+ **但是对于webpack本身来说，对于这些转化是不支持的。**
+ 那怎么办呢? **给webpack拓展对应的`loader`就可以啦** 

loader使用步骤：

+ 步骤一： 通过npm 安装需要使用的loader
+ 步骤二：在`webpack.config.json`中的modules 关键字下进行配置

大部分的loader我们都可以在webpack的官网中找到，并且学习对应的用法。

### css文件处理

##### 准备工作

项目开发过程中，我们必然需要添加很多的样式，而样式我们往往写到一个单独的文件中

+ 在src目录中，创建一个css 文件，其中创建一个`normal.css`文件
+ 我们也可以重新组织文件的目录结构，将零散的js文件放在一个js文件夹中。

在入口文件中引用

```javascript
// main.js
require('对应的位置/css/normal.css')
// 然后重新打包
```

**此时就会报错 告诉我们需要有对应的loader**

#### css-loader

在webpack官网中， 可以找到关于样式的loader使用方法

```shell
npm install --save-dev css-loader ## 安装 css-loader
```

`webpack.config.json`

```json
module.exports = {
  module: {
      // 在rules 这个数组里配置 loader 
    rules: [
      {
        test: /\.css$/,
        use: [ 'style-loader', 'css-loader' ]
      }
    ]
  }
}
```



按照官方配置的`webpack.config.json`,会发现有一个style-loader。

##### style-loader

除了css-loader 我们还需要一个style-loader

```shell
npm install --save-dev style-loader ## 安装 
```

配置 `webpack.config.json`

```json
module.exports = {
  module: {
      // 在rules 这个数组里配置 loader 
    rules: [
      {
        test: /\.css$/,
        use: [ 'style-loader', 'css-loader' ] // css 和 style的位置固定
      }
    ]
  }
}
```

其中 css-loader 是用来解析 css 文件的 style-loader 是用来将样式添加到DOM中的

但是上面 **style-loader 必须要写在 css-loader前面 **

是因为webpack 读取使用loader 的过程中，是按照从右往左的顺序读取的。

##### less-loader

其实和上面一个的都是安装 和 配置 

1. 先创建一个less 文件 依然放到 css文件夹

2. 继续在官网中找到less-loader的相关使用说明

3. 安装 less-loader （注意 还需要安装less 因为webpack需要使用less对less文件进行编译）

   ```shell
   npm install --save-dev less-loader less 
   ```

4. 修改对应的配置文件 

   添加一个rules 选项，用于处理 .less 文件

   ```json
   { 
   test: /\.less$/,
   use: [{
       loader: "style-loader" // creates style nodes from JS strings
       }, {
       loader: "css-loader" // translates CSS into CommonJS
       }, {
       loader: "less-loader" // compiles Less to CSS
       }]
   }
   ```

#### 图片文件处理 

图片资源处理分两种 

+ 小于 8kb 的分为一类
+ 大于 8kb 的分为一类

使用两个loader

+ url-loader  用于处理 小于8kb的文件

+ file-loader 用于处理 大于8kb的文件

  **其中  8kb 的标准可以从配置中修改，但是一般不改**

##### url-loader 

+ 安装

  ```shell
  npm install --save-dev url--loader
  ```

+ 配置

  ```json
  {
          test: /\.(png|jpg|gif)$/,
          use: [
            {
              loader: 'url-loader',
              options: {
                limit: 8192 // 这里就是 设置 8kb限制的地方
              }
            }
          ]
        }
  ```

这也是limit属性的作用，当图片小于8kb时，对图片进行base64编码

##### 大于 8kb 的使用 file-loader 

+ 安装

  ```shell
  npm install --save-dev file-loader
  ```

+ 配置

  ```json
  {
          test: /\.(png|jpg|gif)$/,
          use: [
            {
              loader: 'file-loader',
              //
              options: {
                  limit: 8192,
                  name:'img/[name].[hash:8].[ext]'
                  //在option中添加的选项设置新生成的图片存放位置和指定文件名
              }
            }
          ]
        }
  ```

  

我们发现webpack自动帮我们生成了一个非常的名字

+ 这一个32位hash值，目的是防止名字重复
+ 但是真实开发中，我们可以能对打包的图片名字有一定的要求
+ 比如，将所有的图片放到一个文件夹中，跟上图片原来的名称，同时也要防止重复、

所以我们可以在options中添加选项：

+ `img`：文件要打包到的文件夹
+ `name`：获取图片原来的名字，放在该位置
+ hash8：为了防止图片名称冲突，依然使用hash，但是我们只保留8位
+ ext，使用图片原来的拓展名 

但是，我们发现图并没有显示出来，这事因为图片使用的路径不正确

+ 默认情况下，webpack会将生成的路径直接返回给使用者

+ 但是我们整个程序是打包在dist  文件夹下的，所以这里需要路径下再添加一个dist

  ```js
   // 出口，通常是一个对象，里面至少包含两个重要属性，path 和 filename
      output:{
          path: path.resolve(__dirname,'dist'),// 注意 path通常是一个绝对路径
          filename: 'bundle.js',
          public: 'dist/'
      }
  ```

  

### ES6语法处理

如果需要将ES6语法 转成ES5  一样需要使用 loader

+ 安装

``` shell
npm install --save-dev babel-loader@7 babel-core babel-preset-es2015
```

+ 配置

```js
   {
      test: /\.js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env']
        }
      }
    }
```



## webpack中配置Vue

### 1. 安装 

+ npm 安装 

```shell
npm install vue --save ## 可以指定版本 不需要加 dev 因为运行时也需要
```

### 2. 配置

Vue在开发是发布了两个版本 

1. runtime-only ->  代码中不可以有任何template
2. runtime-compiler -》 代码中可以有template 因为有compiler 可以用于 编译template

修改webpack配置 

```js
resolve:{
    alias:{
        'vue$':'vue/dist/vue.esm.js'
    }
}
```



> ！ 遇到报错
>
> vue-loader有很多版本  从14.之后就需要自己配置一个插件 
>
> 可以手动改一下版本 改回到13.0.0以上的版本 用 `^13.0.0` 

## plugin的使用

plugin是什么

+ plugin是插件的意思，通常是用于对某个现有的架构进行扩展
+ webpack中的插件，就是对webpack现有的功能的各种扩展，比如打包优化，文件压缩等等。

loader和plugin的区别

+ loader主要用于转换某些类型的模块，它本身是一个转换器。
+ plugin是插件，它是对webpack本身的扩展，是一个扩展。

plugin是使用方法： 

+ 步骤一： 通过npm安装需要使用 的plugin(某些webpack以及内置的插件不需要安装)
+ 步骤二： 在webpack.config.js中的plugins中配置插件



##### 添加版权的plugin

用来给打包的文件添加版权声明

+ 该插件名字叫做 `BannerPlugin`,属于webpack自带的插件

在`webpakc.config.js`文件配置

```js
const webpack require('webpack')

moudule.exports = {
    ...
    plugins:[
        new webpack.BannerPligin('这里写版权信息')
        new webpack.BannerPligin('最终版权归谁所有')
    ]
}
```

##### 打包HTML的 plugin 

目前，我们的index.html文件是存放在项目的根目录下的。

+ 我们知道，在真实发布项目时，发布的是`dist`文件夹中的内容，但是dist文件夹中如果没有`index.js`文件，那么打包的js等 文件也就没有意义了
+ 所以我们需要将 `index.html`文件打包到 `dist`文件夹中，这个时候就可以使用 `HtmlWebpackPulgin`插件

`HtmlWebpackPulgin`插件可以为我们做这些事情

+ 自动生成一个`index.html`文件 **可以指定模板生成**
+ 将打包的js文件，自动通过 `script`标签插入到body 中

安装 `HtmlWebpackPulgin` 插件

```shell
npm install html-webpack-plugin --save-dev 
```

使用插件，修改`webpack.config.js` 文件 plugins

```js
plugins:{
 	new htmlWebpackPlugin({
        template: 'index.html',
        // 这里的template 表示要根据什么模板来生成index.html
        // 另外，我们需要删除之前在output中添加的publiuc 属性
        // 否则插入的script标签中的src可能会有问题
    })
}
```

##### JS压缩的Plugin

在项目发布之前，我们必然需要对js等文件进行压缩处理

+ 这里，我们就对打包文件进行压缩
+ 我们使用一个第三方插件 `uglifyjs-webpack-pubgin` ,并且版本号指定 1.1 .1，和CLI2保持一致

```shell
npm install uglifly-webpack-plugin@1.1.1 --save-dev
```

修改`webpack.config.js` 文件，使用插件：

```js
const path = require('path')
const webpack = require('webpack')
const uglifyJsPlugin = require('uglifyjs-webpack-plugin')

module.exports = {
    ...
    plugins:[
        new webpack.BannerPlugin = ('最终版权归AA所有')
    	new uglifyJSPlugin() 
    ]
}
```





## 搭建本地服务器

webpack提供了一个可选的本地开发服务器，这个本地服务器基于node.js，内部使用express框架，可以实现我们让浏览器自动刷新显示我们修改的效果。

不过它是一个单独的模块，在webpack中使用之前需要安装它

```shell
npm install --save-dev webpack-dev-server@2.9.1
```

`devserver`也是作为webpack中的一个选项，选项本身可以设置如下属性：

+ `contentBase`: 为哪一个文件夹提供本地服务，默认是根文件夹，我们这里要填 ` './dist' `
+ `port` : 端口号
+ `inline` : 页面实时刷新 (true/flase)
+ `historyApiFallback` :  在SPA 页面中 依赖 HTML5 的 history 模式

`webpack.config.json`配置如下

```js
devServer:{
    contenBase: './dist',
    inline: true,
}
```

我们还可以在`package.json` 配置一个 `scripts`

```js
"dev" : "webpack-dev-server --open" // --open表示直接打开浏览器
```



## 关于抽离配置的补充

打包设置的抽离 因为开发与运行时配置不同 有的配置开发用 有的配置上线采用 所有需要将其抽离分开