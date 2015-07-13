title: "Yoeman Simple Guide"
date: "2014-05-12"
excerpt: true
comments: true
tags:
    - Yoeman
---

## Intro

Yoeman是一个帮助开发者快速构建一个完整前端项目的工具。俗话说万事开头难，Yoeman可以帮助开发者把开头的事情都做好，让开发者专注于业务逻辑的实现，而非项目本身的搭建过程。

Yoeman里面实际上细分成了三个模块

*   `yo`: Yeoman的项目框架搭建工具
*   `bower`: 包管理工具
*   `grunt`: 构建工具

这三个模块相互独立，由不同的社区进行开发和维护，他们协同工作，使得构建一个完整项目变得简单而又自动化。

## Installation

需要通过`npm`来安装该工具，而`npm`是`Node.js`的包管理工具。安装好`Node.js`后回自动集成`npm`。`Node.js`的安装非常简单，直接查看[官网](http://nodejs.org/)进行安装。

<!-- more -->

###1. 安装`yo`工具

```
npm install -g yo
```

利用`1.2.10+`版本的`npm`安装`yo`的时候会自动安装`grunt`和`bower`

###2. 因为不同的前端项目会用到不同的框架和库，因此需要安装针对性的前端框架生成器，以下举例两个前端框架作为参考

```
#该框架会生成默认前端框架，基于HTML5，包含jQuery, Modernizr, and Bootstrap，可以在安装的时候选择包含哪些内容
npm install -g generator-webapp

#该框架会生成基于angularJS的前端框架项目
npm install -g generator-angular
```

###3. 构建项目

```
yo angular
#会提示是否使用Saas，如果不需要，选择`n`即可
[?] Would you like to use Sass (with Compass)? (Y/n) n
#会提示是否包含Bootstrap框架，一般都是需要用到的，选择y即可
[?] Would you like to include Bootstrap? (Y/n) y
#这里会让选择哪些独立angular模块需要包含进项目，根据需求进行选择
[?] Which modules would you like to include? (Press <space> to select)
❯⬢ angular-resource.js
 ⬢ angular-cookies.js
 ⬢ angular-sanitize.js
 ⬢ angular-route.js
```
选择完回车就开始进行项目构建。

注：由于国内蛋疼的防火墙，安装会出现不稳定的状态，有时候甚至会失败，可以利用goAgent来进行安装加速(npm用代理同理)。也可以通过更改npm为国内源来提升项目搭建速度和稳定性，具体google之。

```
#需要开启goAgent
yo angular --proxy http:127.0.0.1:8087
```

构建完的项目包含以下几个核心内容
```
.
├── Gruntfile.js            #grunt核心脚本
├── app/                    #项目主目录
│   ├── bower_components/   #项目用到的库目录，由bower进行维护
│   ├── fonts/              #存放项目中用到的字体
│   ├── images/             #存放项目中用到的图片
│   ├── index.html          #项目主页文件
│   ├── scripts/            #存放js文件
│   ├── styles/             #存放css文件
│   └── views/              #存放view层模板
├── bower.json              #bower配置文件
├── karma-e2e.conf.js       #karma配置文件，用于测试
├── karma.conf.js           #karma配置文件，用于测试
├── node_modules/           #grunt, bower等工具存放的位置
├── package.json            #npm配置文件
└── test                    #存放测试文件及配置
```

###4. 启动项目

```
grunt server
```
然后直接在浏览器输入localhost:9001即可浏览项目，9001是默认端口，可以在Gruntfile.js中进行修改，如果想修改成80端口，Linux/Mac系统中需要用管理员身份来启动项目，即

```
sudo grunt server
```

###5. 自动化测试

```
grunt test
```

###6. Bower介绍

Bower是最好的web项目包管理工具，可以轻松管理项目中用到的第三方库，以安装和更新jquery-ui为例说明bower的

```
# 列出项目中已经安装的库及对应版本以及最新版本
bower list

# 搜索Jquery-ui
bower search jquery-ui

# 安装jquery-ui, bower会自动安装jquery-ui到bower_components下
# 并在index.html中引用该库文件 
bower install jquery-ui

# 更新Jquery-ui
bower update jquery-ui
```

###7. 发布到Github上

我们可以将yoeman创建的项目发布到github上

简单的流程如下：

*   在Github上创建一个项目
*   clone到本地来
*   复制yoeman创建的项目的文件到clone的项目中
*   在.gitignore中添加
    >node_modules
*   commit并push

因为node_modules是项目工具，bower_components是项目依赖库，总共 大约100MB左右，这个是不需要上传的。

但是再次clone下来的项目需要执行
*   `npm install`来安装项目所需的工具
*   `bower install`来安装项目所需的依赖库
*   然后再执行`sudo grunt server`来启动项目










