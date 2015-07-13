title: "windows下安装和运行midway"
date: "2015-03-23"
excerpt: true
comments: true
tags:
    - midway
---

### 前言

虽然推荐在Mac下进行基于Node的项目开发工作(很多的node模块对windows的支持并不是很友好)，但不可避免的还是存在一些小伙伴需要在Windows的开发环境下进行工作，因此还是需要解决Windows下遇到的各种问题，本文主要是安装指南以及问题和解决方案的汇总，未来也会长期维护和更新。

### 依赖环境搭建

1.  安装Nodejs(>0.11.7)
2.  安装Python(2.7.x, 目前不支持3.x.x)
        *   如果出现`command not found`错误，则需要手动添加系统变量
            *   在系统变量 `PATH` 中添加 `C:\Python27\`
            *   创建 `PYTHONPATH` 中添加 `C:\Python27\`
        *   如果安装了多个版本的Python, 需要通过以下命令指定npm使用的python版本: `$ npm config set python C:/Python27`
3.  安装Visual Studio C++ 2010/2012/2013
    1.  对于一些64位的node和原生模块，也需要安装[Windows 7 64-bit SDK](http://www.microsoft.com/en-us/download/details.aspx?id=8279)
    2.  如果安装失败，试着卸载之前可能安装过的 C++ 2010 x84&x86 Redistributable
    3.  如果64位 compilers 安装过程中失败，可能还需要安装 [compiler update for the Windows SDK 7.1](http://www.microsoft.com/en-us/download/details.aspx?id=4422)
    4.  安装前最好先清理一下硬盘空间，Visual Studio 2012大概需要5G空间，Visual Studio 2013大概需要8G空间。
    5.  安装Visual Studio一定要耐心...周围5米内不要放置任何尖锐物品
    6.  Express版本也可以，不要安装for web版，Visual Studio 2013社区版目前来看也可以。
    7.  由于Visual Studio是收费软件，如果不想付费的话就选择试用，主要是用到了里面的一些编译器工具，所以即使软件过期，应该也不影响环境的搭建。(试用期还没过，仅是理论上的猜测)
4.  设置msvs_version
    1.  设置安装的visual studio的版本，如果安装的是2012，则执行以下命令: `$ npm config set msvs_version 2012 --global`
5.  安装tnpm: `$ npm install -g tnpm --registry=http://registry.npm.alibaba-inc.com`

### Midway安装

*   安装midway `$ tnpm install @ali/midway-toolkit@latest -g`
*   如果出现安装错误，请查看是否完成了上述所有依赖环境的搭建
    *   出现 `python command not found`， 请检查依赖环境中`2`是否安装成功
    *   出现 `error MSB8007`，请检查依赖环境中`3.1`和`3.3`是否安装成功
    *   出现 `error MSB3411`，请检查依赖环境中`3`是否安装成功
    *   出现 `error MSB4019`，请检查64位环境下SDK是否安装及compiler是否更新。
    *   出现 `Cannot read property 'emit' of null` 的错误，目前还木有解决，不过不影响midway的使用(有可能是Visual Studio 2013社区版导致的)

### Midway项目初始化

*   执行 `$ midway init my_app` 进行项目初始化
*   `$ cd my_app` 进入项目目录
*   `$ tnpm install` 安装项目依赖库
*   `$ tnpm start` 启动项目
    *   项目默认监听端口为6001, 启动项目后可以通过`http://localhost:6001/`查看
    *   项目默认为开发环境(local)，如果启动的时候发现是生产环境(production)，可以通过以下命令来指定环境：`$ NODE_ENV=local tnpm start`