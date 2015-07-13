title: "什么是Javascript中的Scope？"
date: "2015-03-18"
excerpt: true
comments: true
tags:
    - javascript
    - scope
---

几乎有所程序语言都具备的特性就是：使用变量来存储、遍历和修改值。但是我们通常没有思考过以下这些问题：

*   变量存储在哪里？
*   当程序需要的时候，变量是如何被寻找到的？

实际上，在设计语言的过程中，为了解决以上的问题，需要定义很多的规则，我们将这些制定和执行这些规则的部分统一起来成为域(Scope)。

一直以来，JavaScript被定义为动态、解释型语言，实际上他是一门编译型语言，只不过与传统的编译型语言相比，JS的编译并不是很完整，并且编译的结果可以运行在很多不同的环境中。即使如此，与传统编译型语言相比，JS引擎仍然使用很多复杂的方式来达到许多相同的编译过程。

传统编译型语言在执行程序前主要进行以下是哪个步骤：

<!-- more -->

1.  词法分析(Lexing)：将代码分成小片段的过程叫做词法分析。举例来说，`var a = 2`会被分成`var`, `a`, `=`, `2`。
2.  语法分析(Parsing)：将一系列的小片段转换成复杂的能表达程序语法结构的元素树。该树被称为AST(Abstract Syntax Tree)
3.  代码生成(Code-Generation)：主要是将AST转换成可以执行的代码。该步主要依赖于何种语言以及何种平台。

实际上，JavaScript引擎与其他编译器一样，编译过程远不止如上三步。除此之外，JavaScript引擎在代码执行前只有很短的编译时间(一般在微秒级)，为了保证非常快的性能，JS引擎使用了各种技术(比如JITs，其主要进行懒编译，甚至动态重新编译等)。

在程序处理的过程中，JS引擎负责整个编译过程和代码的执行。编译器负责编译过程中的所有底层的词法和语法解析，并生成可执行代码。而Scope负责收集和维护一个定义过变量的列表，并强制使用一系列严格的规则来决定被执行的代码是否可以访问这些变量。

举例来说，当我们看到程序`var a = 2;`，我们认为其是一条代码语句。而实际上JS引擎看到的是两条不同的语句，一条是在编译过程中编译器需要处理的语句，而另一条是在执行过程中引擎需要处理的。

编译器处理该语句的简要流程为：

1.  编译器询问Scope该变量是否在特定的Scope集合中已经存在。如果存在，编译器则忽略该定义，否则编译器请求Scope在集合中定义名为`a`的变量。
2.  之后编译器为引擎生成可执行代码。该代码在运行时首先询问Scope在当前的scope集合中是否存在一个叫做`a`的变量可以访问，如果不存在，引擎会到别的域集合中继续寻找(nested Scope)

如果引擎最终找到了该变量，则会进行赋值操作，否则引擎会在执行代码时抛出一个错误。

总结来说：编译器负责判断是否请求Scope在特定集合中定义该变量，而引擎负责通过Scope来寻找特定集合中定义的该变量并执行查询或赋值操作。

通过该代码，我们来看一下引擎和Scope之间是如何对话的。

```javascript
var a = 2;
var a;
alert(a);
```

>Compiler: Hey Scope, 在当前你的集合中是否存在一个叫做a的变量？
>Scope: 等等，我查一下，没有唉~~
>Compiler: 这样啊~ Scope兄，麻烦你帮我定义一下这个变量呗。
>Scope: 好嘞~~

>Compiler: Hey Scope, 在当前你的集合中是否存在一个叫做a的变量？(比较健忘)
>Scope: 等等，我查一下.... 你不是刚刚定义过吗？
>Compiler: 好吧...那当我没说... 闪了~~
>Scope: >_<

Compiler 苦逼的开始生产可执行代码，然后交给了Engine

>Engine: Hey Scope, 我这里有一个变量a，你听说过吗？
>Scope: 我知道的，Compiler刚刚定义过一个。
>Engine: 太棒了，非常感谢，我现在把这里的a赋值为2。

>Engine: Hey Scope, 我这里有一个函数叫做alert，你听说过吗？
>Scope: 听说过啊，他就是Window这个集合里面的成员。
>Engine: 太好了，我要使用它，谢谢啦。
>Engine: 等等，还有变量a你听说过吗？(鱼的记忆，转眼就忘记自己问过的问题了)
>Scope: 我知道啊，你不是刚给他赋值了吗？
>Engine: 额...好吧。

那么接下来，大家可以想象一下，这里面Scope、Compiler和Engine是怎么对话的呢？

```javascript
var b = 10;
function foo(a) {
    var b = a + 1;
    console.log(b);
}
foo(b);
```

```
var value1 = 0, value2 = 0, value3 = 0;
for ( var i = 1; i <= 3; i++) {
    var i2 = i;
    (function() {
        var i3 = i;
        setTimeout(function() {
            value1 += i;
            value2 += i2;
            value3 += i3;
        }, 1);
    })();
}
setTimeout(function() {
    console.log(value1, value2, value3);
}, 100);
```