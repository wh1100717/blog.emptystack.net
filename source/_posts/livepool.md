title: "利用livepool实现请求的截获及转发"
date: "2014-08-02"
excerpt: true
comments: true
tags:
    - livepool
---

前端开发过程中，存在这样一种场景：后端开发人员已经部署好服务器，前端需要提交vm并进行数据连调。

连调过程中的数据请求通过ajax的方式来获取，因为是异步请求数据，目前大部分的请求不支持jsonp跨域方式调用，因此数据只能从服务器端提供。问题就在于当后端只是定义好接口，但并没有对应接口的数据实现时，前端开发只能等待。

举例来说：
当后台部署成功以及vm实现时，前端开发需要通过`http://jutiyewu.alimama.com/module/page1.htm`访问对应的页面，而对应页面如果需要进行数据连调时，具体的ajax访问接口则是通过`http://jutiyewu.alimama.com/module/woshiajax`来完成。因为浏览器跨域限制，不能更改域，因此我们不能直接在本地构造假数据来完成数据连调测试。

这使得前端的开发工作完全依赖并等待后端开发的进度。

通过使用livepool和tianma，可以截获请求构造假数据的方式来解决以上提到的问题，类似的替代方案可以通过fiddler来实现，但是fiddler只支持windows平台，而livepool是跨平台方案。

#### livepool

LivePool 是一个基于 NodeJS，类似 Fiddler 支持抓包和本地替换的 Web 开发调试工具，是 Tencent AlloyTeam 在开发实践过程总结出的一套的便捷的 WorkFlow 以及调试方案。

*   github项目地址：https://github.com/rehorn/livepool
*   安装过程：`$ npm install livepool -g` (需要安装nodejs，具体nodejs安装过程请参考[官网安装说明](http://nodejs.org/))
*   运行livepool: `$ livepool`

其实livepool的运行原理与goAgent类似，它在后台启动一个8090端口的server，浏览器或者全局设置代理，使得浏览器发出的请求经过livepool，livepool对其进行监控和更改。除此之外，其还启用了一个8002的web server，就是其对应的网页管理控制台。

*   因此，启用了livepool以后，如果需要对浏览器进行代理配置。建议使用chrome的[switchysharp](https://chrome.google.com/webstore/detail/proxy-switchysharp/dpplabbmogkhghncfbfdeeokoefdjegm)插件(goAgent也使用的这款插件做vpn代理)。
*   打开管理控制台 http://127.0.0.1:8002

界面说明：

![Alt text](/images/livepool-1.png)

1.  菜单区
2.  Session（显示所有http请求信息）
3.  TreeView（使用树状结构显示Session信息）
4.  功能Tab: Pool（按照项目管理本地替换规则）
5.  功能Tab: Inspector (session查看器，查看请求header，body等信息)
6.  功能Tab: Composer（http请求模拟器，可以模拟http get/post请求）
7.  功能Tab: Filter（session过滤器，根据规则过滤session，只保留关注的）
8.  功能Tab: Log（日志显示）
9.  功能Tab: Timeline（session时间轴，comming soon）
10. 功能Tab: Statics（统计，对站点性能进行评估，comming soon）

#### 路由转发

![Alt text](/images/livepool-2.png)

可以设置rule来实现路由转发，如图所示，其会匹配对应请求，并根据对应的action进行转发。比如匹配请求路径是`http://baidu.com/module/test`，action为`127.0.0.1`，则实际请求地址为`http://127.0.0.1/module/test`。这样我们就可以在本地搭建服务器响应对应请求来完成截获和转发了。

具体利用[tianma]构造假数据的文档请参考[利用tianma本地构造假数据](https://app.yinxiang.com/shard/s27/sh/a2aba3bc-8e4f-4df2-bd0e-b4e62b08f5d5/2b80cb81382e261f264d10c6870b7ecc)。

当然，livepool还有很多别的功能，比如说可以替换资源文件：

![Alt text](/images/livepool-3.png)

如图所示，我们可以很方便的把线上的js/css等文件用我们本地的文件进行替换(支持正则表达式)。也就是说我们可以利用本地项目中的静态文件直接对线上发布的项目进行调试，从而达到不需要本地搭建环境、配置和本地及集成测试的情况下对bug进行精确修改。

当然还有很多别的功能，小伙伴们如果有兴趣可以自行研究。



