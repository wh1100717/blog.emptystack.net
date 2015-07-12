title: "利用天马构造假数据"
date: "2014-7-25"
excerpt: true
comments: true
tags:
    - tianma
---

#### 关于天马的用途

目前了解到项目中，天马工具是作为静态文件服务器来使用的，甚至在他的github最新文档中也没有找到其对url请求做动态处理的api。但是从目前的需求角度来说，需要构造假数据来实现一部分前端需求。

简单来说，我更希望编写的页面中如果包含一个ajax请求 `http://localhost/testAjax` 可以返回成功，具体的返回值可以自定义，来构造假数据，比如需要的是 `{success: true}`，那么天马服务器应该可以匹配请求并返回该内容。

通过查看天马的API，获取代码如下：

<!-- more -->

```
tianma
    .createHost({ port: 80 })
        .mount('*.example.com', [
            function (context, next) {
                context.response
                    .status(200)
                    .write('Hello World');
                next();
            }
        ])
        .mount('/', [
            function (context, next) {
                context.response
                    .status(500)
                    .write('Bad Request');
                next();
            }
        ]);
```

也就是说，其实天马可以设定路由规则并返回对应的内容，上例中如果想响应 `http://localhost/testAjax` ajax请求，则增加如下代码即可：

```
        .mount('/testAjax', [
            function (context, next) {
                context.response
                    .status(200)
                    .write('{success: true}');
                next();
            }
        ])
```

也可以对 `context.response` 直接赋值：

```
        .mount('/testAjax', [
            function (context, next) {
                context.response._status = 200
                context.response._body = '{success: true}'
                next();
            }
        ])
```

因此，天马除了作为一个静态文件服务器，还可以构造假数据，来模拟后端接口来进行数据提供。
