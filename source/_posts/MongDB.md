---
title: MongDB
date: 2021-01-13 23:38:12
categories: 
  - 后端
tags: 
  - Node
  - express
  - MongDB
toc: true
---




## 关系型和非关系型数据库

<!-- more -->
### 关系型数据库（表就是关系，或者说表与表之间存在关系）。

+ 所有关系型数据库都需要用过 `sql`语言来操作
+ 所有的关系型数据库在操作之前都需要设计表结构
+ 而且数据表还支持约束
  + 唯一的
  + 主键
  + 默认值
  + feikong

### 非关系型数据库

+ 非关系型数据库非常灵活
+ 有的关系型数据库就是key-value 对儿
+ 但MongDB是长的最像关系型数据库的非关系型数据库
  + 数据库 -》数据库
  + 数据表-》集合【数组】
  + 表记录-》 文档对象

一个数据库中可以有多个数据库，一个数据库中可以有多个集合(数组)，一个集合中可以有多个文档(表记录)

```javascript
{
    qq:{
        user:[
            {},{},{}...
        ]
    }
}
```

+ 也就是说你可以任意的往里面存数据，没有结构性这么一说

## 安装

+ 下载

  + 下载地址： https://www.mongodb.com/download-center/community

+ 安装

  ```shell
  npm i mongoose
  ```

## 配置

将文件所在路径配置到环境变量中

`C:\Program Files\MongoDB\Server\4.4\bin`一般是这条默认位置

此电脑-右键-属性-高级-环境变量- 找到Path 新建一个 确定

## 启动和关闭数据库

启动：

```shell
# mongodb 默认使用mongd 命令所处盘符根目录下的/data/db 作为自己的存储目录
# 所以在第一次执行该命令之前先自己手动建一个 /data/db
mongod
```

如果想要修改默认的数据存储目录： 可以

```shell
mongod --dbpath = 数据存储目录路径 
```

停止：

```shell
在开启服务的控制台，直接Ctrl + c
或者直接关闭开启服务的控制台。
```

【图片丢失】

## 链接数据库

连接：

```shell
# 该命令默认链接本机的 MongoDB 服务
mongo
```

退出

```shell
# 在连接状态输入 exit 退出连接
exit
```

【图片丢失】

## 基本命令

+ `show dbs`

  + 查看数据库列表(数据库中的所有数据库)
+ `db`

  + 查看当前连接的数据库
+ `use 数据库名称`

  + 切换到指定的数据库,（如果没有会新建）
+ `show collextions`
  + 查看当前目录下的所有数据库
+ `db.表名.find()`
  + 查看表中的详细信息

## 在Node中如何操作MongoDB数据库

### 使用官方`MongoDB`包来操作

> http://mongodb.github.io/node-mongodb-native/

### 使用第三方包`mongoose`来操作MongDB数据库

​	第三方包： `mongooes`基于MongoDB官方的`mongodb`包再一次做了封装，名字叫`mongooes`,是WordPress项目团队开发的。

> https://mongoosejs.com/

【图片丢失了】

## 学习指南（步骤）

官方学习文档：https://mongoosejs.com/docs/index.html

### 设计SCheme 发布Model （创建表）

```javascript
// 1. 引包
// 注意： 安装后才能 require使用
const mongoose = require('mongoose');
                         
// 拿到schema 图表
const Schema =mongoose.Schema

// 2.连接数据库
// 指定连接数据后不需要存在，当你插入第一条数据的时候会自动创建数据库
mongoose.connect('mongodb://localhost/test');

// 3.设计集合结构（表结构）
// 用户表
const userSchema = new Schema({
    username:{// 姓名
        type：String，// 设定是字符串型
        required： true // 添加约束，保证数据完整性，让数据按规矩统一
    }，
    password: {
		type: String,
		require: true
	},
	email: {
		type: String
	}
})

// 4.将文档结构发布为模型
// mongoos.model方法就是用来将一个架构发布为model 
//	第一个参数： 传入一个大写的名词单数字符串来表示你的数据库的名字
// mongoose 会自动 将大写名词 的字符串生成 小写复数 的集合名称
// 例如 这里会变成 users 集合名词
// 第二个参数 架构 
// 返回值： 模型构造函数
const User = mongoose.del('User',userSchems)
```

### 添加数据（增）

```javascript
// 5.通过模型构造函数对 User 中的数据进行操作
var user = nwe User({
    username: 'admin',
    password: '123456',
    email:  'xiaochen@qq.com'
});
user.save(function(err,ret) {
    if (err){
        console.log('保存失败')
    }else {
        console.log('保存成功')；
        console.log(ret);
    }
});
```

### 删除(删)

根据条件删除所有

```javascript
User.remove({
    username: 'xiaoxiao'
},function(err,res) {
    if(err) {
        console.log('删除失败')
    }else {
        conselo.log('删除成功')；
        console.log(ret);
    }
});
```

根据条件删一个：

```javascript
Model.findOneAndRemove(conditions,[options],[callback]);
模型构造函数.findOneRemove(条件,[可选参数]，[回调函数])
```

根据id删除一个：

```javascript
User.findByIdAndRemove(id,[options],[callback]);
```



### 更新（改）

更新所有（覆盖全部）： 

```javascript
User.remove(conditions,doc,[options],[callback];
```

根据指定条件更新一个：

```javascript
User.FindOneAndUpdate([conditions],[update],[options],[callback]);
```

根据id更新一个：

```javascript
// 更新  		根据id来修改表单数据
User.findByIdAndUpdate('传入一个id',{
    username: 'junjun'// 修改的内容
},function(err,ret) {
    if (err) {
        console.log('更新失败')；
    } else {
        console.log('更新成功');
    }
});
```

### 查询(查)

查询所有：

```javascript
// 查询所有 
User.find(function(err,ret) {
    if(err) {
        console.log('查询失败');
    }else {
        console.log(ret);
    }
})
```

条件查询所有：

```javascript
// 根据条件查询
User.find({ username :'xiaoxiao' // 这是一段条件
          },function(err,ret) {
    if (err) {
        console.log('查询失败');
    }else {
        console.log(ret);
    }
});
```

条件查询单个： 

```javascript
// 按照条件查询单个，查询出来的数据是一个对象（{}）
// 没有条件查询使用findONE，查询的是表单中的第一条数据
User.findOne({
    username: 'xiaoxiao'
},function(err,ret) {
    if(err) {
        console.log('查询失败');
    } else {
    	console.log(ret);
    }
});
```

