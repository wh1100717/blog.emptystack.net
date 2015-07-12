title: "Nodejs 学习笔记"
date: "2015-01-22"
excerpt: true
comments: true
tags:
    - nodejs
    - 笔记
---

## The Web World is Changing

*   实时web应用的需求急剧增加
*   用户体验变得越来越重要
*   需求频繁，敏捷响应变化，更多的前后端沟通与协作
*   富客户端应用的产生与发展
*   越来越的工具和组件来帮助规范前端开发流程及提高前端开发效率

## What is Nodejs

*   Server-side Javascript
*   异步，事件驱动，非阻塞I/O
*   基于 Chrome V8 Javascript 引擎

## Why Nodejs

*   前端熟悉的语言，学习成本低
*   都是JS，可以前后端复用
*   采用npm进行模块管理和复用
*   事件驱动，非阻塞适合I/O密集型业务
*   执行速度还可以
*   众多基于Nodejs构建的前端开发、调试、部署工具

<!-- more -->

## How to use Nodejs

### 如何安装

身处于天朝环境下，前端开发会受到很大的阻碍，其中包含以下方面：

*   npm(node package manager)访问速度非常之慢
*   bower(js package manager)访问速度依赖于github
*   github访问速度非常不稳定，时快时慢
*   部分资源来自于amazon s3，访问相当不稳定，而且很慢
*   开发过程个可能涉及到node版本切换

具体的解决方案请参考：[快速搭建 Node.js 开发环境以及加速 npm](https://cnodejs.org/topic/5338c5db7cbade005b023c98)

### 阻塞和非阻塞

#### 传统的I/O阻塞式编程

```javascript
var urls = db.query("select * from urls");  //程序处于等待数据库查询的状态
urls.each(function(url){
    var page = http.get(url);   //程序处于等待通过url获取页面内容的状态
    save(page);                 //程序处于等待保存页面内容的状态
})
console.log("hello world");
```

#### 非阻塞式编程

```javascript
db.query("select * from urls", function(urls){
    urls.each(function(url){
        http.get(url, function(page){
            save(page);
        });
    });
});
console.log("hello world");
```

当程序需要进行较长事件的IO操作时，与之相关的操作通过回调函数的方式被挂起，程序并不等待，而是直接执行接下来的语句`console.log("hello world")`, 直到IO操作完成后通过事件驱动来执行对应的回调函数。

### CommonJS

Nodejs中模块 按照[CommonJS](http://www.commonjs.org/specs/)规范进行设计的。

```javascript
//加载全局模块http  
var http = require("http");  
  
//加载当前目录下的duowan.js模块里面的myname  
var myname = require("./duowan").myname  
  
//加载绝对路径/node/modules下面的common.js模块  
var common = require("/node/modules/common")  
```

*   通过require(moduleName)我们可以加载其它的模块
*   如果模块名不含有/或者或者./就会去加载通用模块，如果带有./或者../则按照相对路径规则从当前文件所在文件夹开始寻找模块，如果以/开头则从系统根目录开始找
*   模块引入后可以把整个模块赋予到一个变量中，也可以直接暴露模块中的某个对象或者属性或者方法。其实这点和javascript一样。

```javascript
exports.hello = function () {
    console.log('Hello World!');
};
```

exports对象是当前模块的导出对象，用于导出模块公有方法和属性。别的模块通过require函数使用当前模块时得到的就是当前模块的exports对象。以下例子中导出了一个公有方法。

通过module对象可以访问到当前模块的一些相关信息，但最多的用途是替换当前模块的导出对象。上述可以代码可以改写为：

```javascript
module.exports = {
    hello: function(){
        console.log('Hello World!');
    }
}
```

### 简单的Web Server示例

```javascript
//示例来自于官网
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```

## Frameworks on Nodejs

*   Express
*   Koa
*   Socket.io
*   MEAN
*   Sails

## Other JS Web frameworks

*   Meteor
*   Fibjs
*   io.js

## Upcoming Features for Nodejs

*   ES6
*   Generator
*   co
*   thunkify
*   koa

## 其他参考资料

*   [Nodejs 2014大事件](http://blog.rednode.cn/year-2014-of-node-js/)
*   [Nodejs Documentation](http://nodejs.org/documentation/)
*   [Nodejs包教不包会](https://github.com/alsotang/node-lessons)
*   [Nodejs入门](http://www.nodebeginner.org/index-zh-cn.html)
*   [七天学会Nodejs](http://nqdeng.github.io/7-days-nodejs/)