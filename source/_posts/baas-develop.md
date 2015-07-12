title: "基于BAAS的跨平台实时无后端开发实践"
date: "2015-05-12"
excerpt: true
comments: true
tags:
    - BAAS
    - LeanCloud
    - Firebase
---

设想一下，如果在某些场景下，后端请求可以抽象成固定模型(比如符合RESTful API接口规范)，那么数据的查询、存储、更新、删除操作便完全可以流程化和自动化，我们也不需要额外的投入人力去进行后端的开发，更不需要额外的搭建服务器来响应请求。这种把后端资源作为Service的模式称之为BAAS(Backend As A Service)。

实际上，随着Web技术和移动端技术的发展，开发模式已经发生了很大变化，逻辑层内容完全可以在前端/移动端完成，而后端更多的是提供数据层的支持，但这不意味着后端只是提供简单的数据存储查询接口服务，更多的内容需要后端来支持，比如说提供实时数据广播支持，消息推送支持、自动化邮箱验证支持、自动化授权支持、数据和接口调用分析支持、跨平台多终端支持。很遗憾，就目前来说，大部分的后端项目对于以上新特性支持力度都不大，一方面需要比较多的开发精力，一方面业务需求不同，也并不一定需要。但总的来说，任何新技术需求都需要额外投入开发成本，推动起来难度都很大。

提供BAAS产品服务的公司有很多家，比如国内的[LeanCloud](https://leancloud.cn/), 国外的[Firebase](https://www.firebase.com/)。

以Firebase为例，体验一下BAAS爽快的开发模式。

## Startup

####注册

Firebase的注册非常简单，可以直接用Github账号进行授权注册。

创建完账号以后，需要创建一个App应用。

创建完应用后直接通过`https://<your-firebase>.firebaseio.com/`进入后台管理界面

<!-- more -->

#### 引用Firebase

如果Nodejs服务器使用Firebase的话，可以使用npm来安装firebase的依赖库。如果是前端js中使用的话，可以使用bower来安装firebase的依赖库(webpack/browserify也可以使用npm)。

```
npm install firebase --save
or
bower install firebase --save
```

当在页面中引用了firebase后，需要初始化一个Firebase的引用

```javascript
var Firebase = require("firebase")
var fbRef = new Firebase("https://<your-firebase>.firebaseio.com/")
```

#### 数据存储和修改操作

```javascript
//存数据
var usersRef = fbRef.child("users");
usersRef.set({
    eric: {
        full_name: "Alan Turing",
        hobbies: ["电影"],
        plans: [{
            name: "旅游",
            time: "2016-06-01"
        }],
        likeCount: 100,
        unused: "will be deleted"
    },
    wendy: {
        full_name: "Wendy Liu"
    }
}, function(error){
    //其他的操作也都可以通过回调函数的方式来获取到操作结果
    if(error == undefined){
        console.log("save success")
    }
});
//更新数据
usersRef.child("eric").update({
    full_name: "Eric Wang"
})
//如果数据格式为List，如下操作可以插入一条数据
usersRef.child("eric/hobbies").push("游泳")
//也可以插入一个对象
usersRef.child("eric/plans").push({
    name: "健身"
    time: "2016-07-01"
})
//删除一条数据
usersRef.child("eric/unused").set(null)
//避免多个客户端同时修改文件可以使用如下方法
usersRef.child("eric/likeCount").transaction(function(currentLikeCount){
    return currentLikeCount + 1;
})
```

#### 数据实时读取操作

数据读取方面，Firebase不同于普通服务器，Firebase不仅提供了普通的查询接口，还使用了事件监听类型的读取接口，开发者可以直接通过事件监听的方式来获取到数据库中最新的数据变更，并及时进行数据同步，具体的读取接口类型如下：

*   value:  `value`被用来读取指定内容的静态快照，如果数据发生变更，则对应的回调函数会被执行。

```javascript
usersRef.on("value", function(snapshot){
    console.log(snapshot.val());
}, function(errorObject){
    console.log("The read failed: " + errorObject.code);
})
```

*   child_dded: 与`value`每次返回所有内容不同，`child_added`只有在指定内容中增加child时才会被触发，回调函数中包含新增的元素。

```javascript
usersRef.on("child_added", function(snapshot){
    var newUser = snapshot.val();
    console.log("Name: " + newUser.name);
})
```

*   child_changed: 只有在指定数据的child被更新时才会被触发，回调函数中包含被修改的元素。

```javascript
usersRef.on("child_changed", function(snapshot){
    var changedUser = snapshot.val();
    console.log("Name has been changed to ", changedUser.name);
})
```

*   child_removed: 只有在指定数据的child被删除的时候才会被触发，回调函数中包含被删除的元素。

```javascript
usersRef.on("child_removed", function(snapshot){
    var deletedUser = snapshot.val();
    console.log(deleteUser.name + " has been deleted");
})
```

*   child_moved: 当有序数据数据发生更改时会被触发

如果想要关闭数据监听，可以使用如下方式：

```javascript
//需要传入发起监听对应的监听函数作为originalCallback传入
usersRef.off("value", originalCallback)
//取消所有该类型的监听
usersRef.off("value")
//取消所有的监听
usersRef.off()
```

如果只是想读取一次信息，则可以使用如下方式：

```javascript
usersRef.once("value", function(data){
    console.log(data);
})
```

Firebase也提供了数据筛选、排序等功能，具体内容可以参考[文档](https://www.firebase.com/docs/web/guide/retrieving-data.html#section-queries)

#### Authentication

Firebase提供了如下的授权和认证类型：

*   集成Facebook, Github, Google, Twitter账号授权
*   Email & Password 登陆机制和账号管理
*   Single-session 匿名登陆
*   自定义登陆token(可以集成自己的认证服务器或SSO)

以Email & Password方式为例。

```javascript
//获取用户
var authRef = new Firebase("https://dingo-coder.firebaseio.com/")
//获取登陆信息，如果未登录则为null
console.log(authRef.getAuth())
//用户注册
authRef.createUser({
    email: "test@test.com"
    password: "test"
}, function(error, userData){
    ....
})
//用户登陆
authRef.authWithPassword({
    email: "test@test.com"
    password: "test"
}, function(error, authData){
    ...
})
//注销
authRef.unauth()
//修改邮箱
authRef.changeEmail({
    oldEmail: "test@test.com"
    newEmail: "new_test@test.com"
    password: "test"
}, function(error){
    ...
})
//修改密码
authRef.changePassword({
    email: "test@test.com"
    oldPassword: "test"
    newPassword: "newtest"
}, function(error){
    ...
})
//重置密码，此时Firebase会给注册邮箱发送密码重置邮件
authRef.resetPassword({
    email: "test@test.com"
}, function(error){
    ...
})
```

#### Security & Rules

Firebase的安全和规则设置非常灵活，可以细粒度到数据中每一项的内容。并且具有比较强大的匹配方式。详细内容可以查看[文档](https://www.firebase.com/docs/security/)，这里以**只有登陆用户才有权限获取对应Users表中自己信息**的需求来看一下规则是如何设置的：

```javascript
{
  "rules": {
    "public": {
      ".read": true,
      ".write": true
    },
    "admin": {
      ".read": "auth.uid === 'admin'",
      ".write": "auth.uid === 'admin'"
    },
    "users": {
      "$user_id": {
        ".read": "$user_id === auth.uid",
        ".write": "$user_id === auth.uid"
      }
    }
  }
}
```

如上的配置规则可以看到，其中分为两项：`public`、`admin`和`users`。其中`public`表示任何人对其有读写权限，`admin`表示只有uid为`admin`的用户才有权限,，`users`表示只有对应的`user_id`和`auth.uid`相同的用户才可以访问，及用户只能访问符合该规则的数据。

Rules非常强大，除了做权限控制还有很多别的用处，比如数据Validation、设置index、数据排序方式设置等。


#### What's more?

Firebase还有很多功能，感兴趣的可以详细了解~~

*   Dashboard进行调用监控、分析
*   Android、IOS的SDK
*   REST API 接口
*   Offline数据缓存机制
*   多终端同步机制