title: "LocalDB分享"
date: "2015-03-20"
excerpt: true
comments: true
tags:
    - localdb
---

## 为什么要做LocalDB而不是别的

*   Canvas && SVG ([echarts](http://echarts.baidu.com/index.html))
*   video && audio && preload
*   webGL ([qtek](https://github.com/pissang/qtek))
*   geolocation
*   websocket
*   Storage && indexedDB && postMessage
*   applicationCache

## 为什么需要LocalDB

传统意义上的浏览器是用来进行网页浏览的，但是随着HTML5及前端领域的发展，浏览器承载了更多的角色和内容。

*   利用HTML5可以在浏览器上构建复杂的游戏，比如[Bejeweled], [Angry Birds], [Pirates Love Daisies], 甚至还有多人联网射击游戏[BananaBread]。现在已经出现了为数众多的[游戏引擎]，甚至可以利用WebGL通过硬件加速来渲染[3D效果]。

*   利用HTML5可以在浏览器上构建功能强大的应用，比如图片处理应用[deviantART muro], [个人身体健康及运动监控], [Nobuaki Honma的个人主页]

*   除此之外，随着移动端设备性能的提升，越来越多基于浏览器的手机跨平台应用产生并简化了开发流程。伴随着applicationCache的广泛应用和普及，更多离线常驻浏览器应用会产生和发展。

HTML5提供了三种存储方式：Web Storage, indexedDB 和 applicationCache。

*   indexedDB是基于前端的NOSQL数据库，优点是有索引，浏览器级别高性能查询支持，缺点是接口复杂，使用麻烦，兼容性目前还不理想。

*   applicationCache用于程序离线存储，主要是进行资源文件缓存。

*   webStorage提供简单的键值对存储，使数据可以在同一域的页面中共享，其中localStorage提供的存储即使浏览器关闭了也仍然会存储，真正意义上实现了浏览器数据存储的持久化。

我们再试想一下未来的前端业务场景可能会如何？

*   基于前端用户行为自动化定制个性化网页
*   用户行为数据在多个白名单域中共享
*   用户数据跨终端共享
*   用户数据实时分析及响应
*   用户数据模块化及服务化，域和用户数据模块实现多对多存取关系。

那么问题来了，这些离线或者富客户端应用对于数据的存储和查询的需求到底是怎样的呢？

*   简单的存取接口
*   多种数据格式支持
*   浏览器平台兼容性保证
*   通用性智能存储方案选择
*   多样化数据筛选和查询
*   跨域数据共享
*   多域共享存取的同步问题解决
*   数据存储安全性保障
*   数据存储容量已满时候自动化处理方案

那么LocalDB拥有什么呢？

*   支持多种安装和加载方式，可以直接引用独立文件(无第三方依赖库)，也可以通过bower或者component安装，或者作为requirejs/seajs的模块进行模块化加载。
*   非常非常简单的接口。
*   根据用户的浏览器和需求自动选择最适合的存取方式，智能退化方案，保证基本存储需求的可用性。
*   提供多种数据格式支持，甚至可以直接进行正则表达式或者函数存取操作。
*   提供多样化数据查询、筛选、排序功能。
*   提供域白名单功能，实现跨域数据共享。
*   同一个域可以获得多个LocalDB数据源，实现数据源的模块化和服务化。
*   提供数据高安全性需求，比如数据简单加密功能，通过proxy来隐藏实际存储域。
*   提供数据存储容量已满时候自动覆盖最旧数据以及设置数据过期时间等方式来解决数据容量有限的问题。
*   提供chrome浏览器下设置容量并请求用户确认的功能。

正式因为LocalDB如此多的特性，使得其使用者可以完全脱离HTML5数据存储的底层，在享受HTML5带来的便利的同时，又兼顾了旧的浏览器，在不需要关注数据存储格式的选择时候，用统一的接口完成最优的数据存取操作，甚至可以将数据作为服务模块化并提供给所有白名单中的域使用，实现域与数据之间多对多的存取模式。

## 如何使用LocalDB

http://localdb.emptystack.net/gettingStarted/

## 目前的待做事项

*   支持Promises
*   支持域白名单功能，实现跨域共享数据
*   存储数据简单加密功能
*   多域同时进行localDB数据修改时，增加锁和队列的功能。
*   支持sort排序







[Bejeweled]: http://bejeweled.popcap.com/html5/0.9.12.9490/html5/Bejeweled.html
[Angry Birds]: http://chrome.angrybirds.com/
[Pirates Love Daisies]: http://www.pirateslovedaisies.com/
[BananaBread]: https://developer.cdn.mozilla.net/media/uploads/demos/a/z/azakai/3baf4ad7e600cbda06ec46efec5ec3b8/bananabread_1373485124_demo_package/index.html
[游戏引擎]: http://www.zhihu.com/question/20079322
[3D效果]: http://www.chromeexperiments.com/webgl

[deviantART muro]: http://muro.deviantart.com/
[个人身体健康及运动监控]: http://aprilzero.com/
[Nobuaki Honma的个人主页]: http://rbv.me/