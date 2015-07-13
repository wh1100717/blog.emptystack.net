title: "Web Component 组件化开发初探"
date: "2015-05-20"
excerpt: true
comments: true
tags:
    - Web Components
    - Polymer
    - Dingo
---

### 前端组件化

关于前端组件化的思考和讨论有很多，建议有时间可以阅读一下下面的内容来加深对于前端组件化的理解，这里不展开讨论。

*   [2015前端组件化框架之路](https://github.com/xufei/blog/issues/19)
*   [关于前端开发中“模块”和“组件”概念的思考](https://github.com/hax/hax.github.com/issues/21)

### Welcome To The Future

我认为未来的前端开发会基于两种模式进行：

*   基于Web Component的组件开发: 随着Web Component标准的到来，使通过自定义标签扩展HTML来进行前端组件化成为了可能。Angular 2.0依照Web Component标准进行设计，Reactjs由于使用了Virtual DOM层，也可以很容易的构建Web Component Portal，Polymer 将于今年发布v1.0版本，一些都预示着变革即将到来。
    *   Templates，定义的标记块，不会被渲染但可以随后被激活使用。
    *   Decorators，基于CSS选择器来应用模板，从而对文档进行丰富的视觉和行为的变化。
    *   Custom Elements，让用户自定义新的标签名和新的脚本接口。
    *   Shadow DOM，封装的DOM子树，更可靠的用户界面元素组成。
    *   Imports，定义Templates、Decorators和Custom Elements如何作为资源打包加载。
*   基于Javascript的组件开发: 随着Javascript引擎对于js的解析速度越来越快，类似于Famo.us这样的公司在寻找直接通过JS来构建整个页面的可能性。DOM层扁平化、RenderTree、Physics Animation Engine、Integrated with DOM, Canvas and WebGL, Cross Platform等特性都为这种开发模式带来了生机。由于本文主要是针对Web Component，这里不详细展开。

### Dingo

#### Dingo是什么呢？

Dingo实际上是一个基于Web Component的代码编辑和页面预览的组件（原型和样式风格来自于Famous的代码编辑器）。

<!-- more -->

作者: 卜一 && 仓下

#### 那么在项目中想要引入Dingo需要多少代码呢？

```html
<html>
    <head>
        <script src="http://g.tbcdn.cn/forest/dingo/0.1.0/vendor/webcomponentsjs/webcomponents.min.js"></script>
        <link rel="import" href="http://g.tbcdn.cn/forest/dingo/0.1.0/forest-coder.html">
    </head>
    <body>
        <div style="height:600px">
            <forest-coder></forest-coder>
        </div>
    </body>
</html>
```

实际上只需要三行代码。
*   引入`webcomponents.min.js`文件，用来使浏览器支持Web Components
*   引入 `forest-coder.html`文件，用来支持自定义标签
*   在body中填入`<forest-coder></forest-coder>`用来生成该组件

#### 如果我们自定义一些初始化的内容怎么做呢？

```html
<forest-coder
    html="This is HTML text"
    js="console.log('hello world')"
    css="* {color: red;}"
    title="This is Title"
    icon="Some custom icon URL"
    style="2">
</forest-coder>
```

我们可以直接通过`forest-coder`的属性来进行初始化。这样就可以生成一个完全属于我们个性化的组件了。

#### 如果我们需要动态去更改属性值呢?

我们可以通过如下coffee代码来更改自定义标签的属性值

```coffee
document.addEventListener "polymer-ready", ->
    editor = document.querySelector "forest-coder"
    editor.setAttribute "css", """
    body {
        color: green
    }
    """
    editor.setAttribute "html", """
    <p>This is a paragraph.</p>
    <span>This is a span.</span>
    """
```

如果我们使用Jquery的话，直接使用属性赋值操作即可。

```coffee
$("forest-coder").attr "css", """
body {
    color: green
}
"""
```

### 那么该组件是怎么实现的呢？

forest-coder.html中，自定义组件标签的结构如下：

```html
<link rel="import" href="vendor/polymer/polymer.html">
<polymer-element name="forest-coder" attributes="html css js icon title type">
    <template>
        <link rel="stylesheet" type="text/css" href="css/dingo.css">
        <div id="container">
            ...DOM代码
        </div>
    </template>
    <script>
        ...JS代码
    </script>
</polymer-element>
```

*   首先需要引用polymer.html
*   利用`polymer-element`标签定义name为`forest-coder`的标签，并指定可配置属性为`html css js icon title type`，
*   在`polymer-element`标签内部的`template`中引入组件需要的css代码和html代码。
*   在`script`标签中引入具体的js代码。

具体的js代码结构如下：

```coffee
Polymer "forest-coder", {
    html: ""
    css: ""
    js: ""
    icon:"/img/dingologo.png"
    title: "Dingo"
    layout: 1
    created: ->
        # "An instance of the element is created."

    ready: ->
        # "The <polymer-element> has been fully prepared (e.g. shadow DOM created, property observers setup, event listeners attached, etc)."
        editor = new Editor(@$.container, {
            layout: @layout
        })

    attached: ->
        # "An instance of the element was inserted into the DOM."

    domReady: ->
        # "Called when the element’s initial set of children are guaranteed to exist. This is an appropriate time to poke at the element’s parent or light DOM children. Another use is when you have sibling custom elements (e.g. they’re .innerHTML‘d together, at the same time). Before element A can use B’s API/properties, element B needs to be upgraded. The domReady callback ensures both elements exist."

    detached: ->
        # "An instance was removed from the DOM."

    attributeChanged: (attrName, oldVal, newVal) ->
        # console.log "An attribute was added, removed, or updated. Note: Changed watchers are often easier to use and can watch either ordinary properties or attribute changes matching published properties."
    htmlChanged: (oldVal, newVal) -> editor.set_editor("html", newVal)
    cssChanged: (oldVal, newVal) -> editor.set_editor("css", newVal)
    jsChanged: (oldVal, newVal) -> editor.set_editor("js", newVal)
}
```

利用`Polymer`来创建对应标签的js逻辑部分。标签的创建、初始化、属性变化等都有对应的函数来监听。比如说标签包含属性`icon`，则可以使用`iconChange: (oldVal, newVal) -> console.log(newVal)`对`icon`值发生变化来进行监听。

一个标签的创建分为多个阶段，分别为`created`、`ready`、`attached`、`domReady`、`detached`、`attributeChanged`。具体每个阶段代表什么请查看上述代码的注释部分。

#### 如何获取插件中的DOM节点？

因为Web Component使用Shadow DOM特性来对其DOM进行隐藏。使其不能直接访问其DOM节点。使用Polymer如何获取对应的节点呢？

```coffee
Polymer "forest-coder", {
    created: ->
        #通过this.$ + template中节点id的方式获取该节点
        console.log this.$.container
}
```

如上述代码表示在Template中有一个id为container的元素，我们可以在js代码中通过`this.$.container`来获取该元素节点。

#### 是否可以使用Jquery来操作组件中的DOM呢？

我们无法通过`$("#id")`的方式来访问组件中的DOM元素。而需要利用`$.this.id`来对获取对应的节点。因此实际上我们也是可以使用Jquery的，方法如下：

```coffee
Polymer "forest-coder", {
    created: ->
        #我们在Template中最外层设置一个id为container的容器
        container = this.$.container
        #将获取到的节点作为参数传递给Jquery
        container = $(container)
        #我们就可以利用该container来操作组件中的元素了
        console.log contianer.find("input#userName").val()
```

### 双向绑定

我们实际上可以在`template`中进行数据的双向绑定。及标签的属性值一旦放生变化，对应`template`中绑定了该属性值的数据也会跟着变化。

如下代码中将组件的icon属性在`template`中通过`{{icon}}`的方式进行了绑定。

```html
<link rel="import" href="vendor/polymer/polymer.html">
<polymer-element name="forest-coder" attributes="html css js icon title type">
    <template>
        <link rel="stylesheet" type="text/css" href="css/dingo.css">
        <div id="container">
            <a href="" target="_blank" class="item">
                <img src="{{icon}}" id="logo">
            </a>
        </div>
    </template>
    <script>
        ...JS代码
    </script>
</polymer-element>
```

至此，Dingo用到的一些技术细节就介绍完了，Web Component 的特性还有很多，而Polymer也还有很多的使用方式可以去探索，以下是一些不错的资源可供参考：

*   [W3C Web Components](http://www.w3.org/html/ig/zh/wiki/Web-Components)
*   [Custom Elements](https://customelements.io/)
*   [Polymer v0.5](https://www.polymer-project.org/0.5/)
*   [Road To Polymer](http://chuckh.github.io/road-to-polymer/)
*   [Web Components the Right Way](https://github.com/mateusortiz/webcomponents-the-right-way)

