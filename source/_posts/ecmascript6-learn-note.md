title: "ECMAScript 6 学习笔记"
date: "2015-04-13"
excerpt: true
comments: true
tags:
    - ECMAScript
    - 笔记
---

## 介绍

ECMAScript 6 是 ECMAScript 标准即将到来的版本。该标准计划于2015年7月批准并实施。相比于在2009年更新的ES5标准，ES6对Javascript语言进行了一系列非常重要的更新。主要的js引擎对标准中的新特性的实现已经在进行中了。

想要查看ES6全部的新特性描述点击 [这里](https://people.mozilla.org/~jorendorff/es6-draft.html)

大部分内容来自于 [es6features](https://github.com/lukehoban/es6features)

ES6 包含以下新特性：

*   arrows
*   classes
*   enhanced object literals
*   template strings
*   destructuring
*   default + rest + spread
*   let + const
*   iterators + for..of
*   generators
*   unicode
*   modules
*   module loaders
*   map + set + weakmap + weakset
*   proxies
*   symbols
*   subclassable built-ins
*   promises
*   math + number + string + object APIs
*   binary and octal literals
*   reflect api
*   tail calls

## 尝试ECMAScript 6

目前现代浏览器部分支持ES6特性，如果想进行测试，可以使用以下方式：

*   Google Traceur, 使用Chrome插件 [ES6 Tepl](https://chrome.google.com/webstore/detail/es6-repl/alploljligeomonipppgaahpkenfnfkn), 安装后可以进行ES6测试。
*   安装 `0.11.x` 版本的nodejs，并执行 `node --harmony` 进行调试。

## ECMAScript 6 特性

### Arrows 箭头函数

Arrows 实际上使用 `=>` 对函数书写进行了简化。语法上与C#, Java 8 和 CoffeeScript 类似。 

需要注意的是：

*   函数体内的this对象，绑定定义时所在的对象，而不是使用时所在的对象。
*   不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
*   不可以使用arguments对象，该对象在函数体内不存在。

```javascript
arr = [1,2,3,4,5,6,7,8,9,10,11]

arr1 = arr.map(n => n + 1)
console.log(arr1); // [2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ]

five = [];

arr.forEach(v => {
    if(v % 5 === 0){
        five.push(v);
    }
});
console.log(five); //[5, 10,15]

//Lexical this
var bob = {
    _name: "Bob",
    _friends: [],
    printFriends() {
        this._friends.forEach(f => console.log(this._name + " knows " + f));
    }
}
bob._friends.push("Eric");
bob.printFriends()  //Bob knows Eric
```

### Classes 类

ES6 的 classes 是基于原型的OO(面向对象)模式的简单语法糖封装。现在提供原生的Class支持后，对象的创建，继承更加直观了，并且父类方法的调用，实例化，静态方法和构造函数等概念都更加形象化。

```javascript
//Person类
class Person {
    //构造器
    constructor(name){
        this.name = name;
    }
    //实例方法
    sayHi(){
        console.log("Hi, this is ", this.name);
    }
}
var eric = new Person("Eric Wang");
eric.sayHi();   //Hi, this is Eric Wang

//类的继承
class Engineer extends Person {
    constructor(name, position) {
        super(name);
        this.position = position;
    }
    introduceWork() {
        console.log("I'm doing the ", this.position, " engineer work.")
    }
}

var xiaxing = new Engineer("Xia Xing", "Software");
xiaxing.sayHi();            // Hi, this is  Xia Xing
xiaxing.introduceWork();    //I'm doing the  Software  engineer work.
```

### Enhanced object literals 增强的对象字面量

对象字面量被扩展为可以支持对象定义时进行设置原型，定义方法和调用富函数。

```javascript
var person = {
  __proto__: {
    eat(){
      console.log("eat something!");
    },
    sleep(){
      console.log("sleep at night~");
    }
  },
  work(){
    console.log("working....");
  }
}

person.eat()    //eat something!
person.sleep()  //sleep at night~
person.work()   //working...
```

### Template String 模板字符串/字符串插值

模板字符串提供了构造字符串的语法糖，其与Perl, Python, Coffeescript中的语法类似。通过反引号\`来构造具有插值功能的字符串，其中可以包含以`${expression/variable}`构造的表达式或变量。

```javascript
// 基本的字符串构建
`In JavaScript '\n' is a line-feed.`

// 多行字符串构建
`In JavaScript this is
 not legal.`

// 创建一个差值字符串
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`
```

### Destructuring 解构

支持对数组和对象的绑定进行模式匹配，并自动解析其中的值。解构具有类似于对象查询(`foo["bar"]`，如果值不存在则返回`undefined`)的容错模式。

```javascript
//数组匹配
var [a,,b] = [1,2,3];
console.log(a, b);  //1 3

// 对象匹配
var {b:b, c:c, d:d} = {
  b: {name: "Eric"},
  c: [1,2,3],
  d: function(x){return x+1;} 
}
console.log({}.toString.call(b), b.name);
console.log({}.toString.call(c), c[1]);
console.log({}.toString.call(d), d(10));

// 也可以用于函数传参中
var g = ({name: x}) => console.log(x);
g({name: "Eric"})

// Fail-soft destructuring 容错解构
var [a] = [];
console.log(a === undefined);   //true

// Fail-soft with default  带默认值的容错解构
var [a = 1] = [];
console.log(a === 1);   //true
```

### Default + Rest + Spread 默认参数，不定参数，扩展参数

```javascript
//默认参数值
var f1 = (x, y=12) => x + y
console.log(f1(3))      // 15
console.log(f1(3, 4))   // 7

//不定参数
var f2 = (x, ...y) => x * y.length
console.log(f2(3), f2(3,1), f2(3,1,"1",true))       //0 3 9

//扩展参数
var f3 = (x, y, z) => x + y + z
console.log(f3(...[1,2,3]))         //6
```

### let + Const 关键字

const为常量，一旦定义不可修改其值，也不可重复定义。

let可以看做块域内的var，区别在其作用域为代码块，并且不能重复定义。

```javascript
let i = 1111
for (let i=0;i<2;i++)console.log(i);//输出: 0,1
console.log(i); //输出：1111
let i = 2222;   //Error: Duplicate declaration, i
```

如果变量没有在外层定义，则会去寻找代码区块中是否已经定义对应变量。

```javascript
for (let i=0;i<2;i++)console.log(i);//输出: 0,1
console.log(i);//输出：2
```

### For..Of 

我们通过`for in`和`for of`的实践来快速了解其功能。

除此之外，`for of` 还可以用在迭代器中。

```
var items = [10,11,12,13]
for (index in items) {
  console.log(index)    //输出0,1,2,3
}
for (value of items) {
  console.log(value)    //输出10,11,12,13
}
```

### Iterator, Generator

*   `iterator`是一个包含返回`{done, value}`元组的叫做`next()`方法的对象。
*   `iterable`是一个包含内部方法的对象，该内部方法形式为`obj[@@iterator]()`，并返回一个迭代器。
*   `generator`是一个特定类型的迭代器(iterator)，其`next()`的结果取决于曲艺对应的`generator function`的行为。`generator`拥有一个`throw`方法，其`next()`方法接收一个参数。
*   `generator function`是一个特殊类型的函数，其充当generator构造器的角色。generator函数体内可以使用上下文关键字`yield`。可以在`yield`关键字出现的地方通过`next`和`throw`向函数体内传递变量和异常。generator函数使用`function*`的语法进行定义。
*   `yield`关键字可以暂停函数的执行，随后可以再进入函数继续执行。
*   `generator comprehension` 是创建generator的简短表达式，例如：`(for (x of a) for (y of b) x * y)`

Generator深入学习可以参考以下文档：

*   [ES6之Generator基础篇](http://www.atatech.org/article/detail/23353/0?rnd=1176158712)
*   [ES6 Iterators, Generators, and Iterables](https://blog.domenic.me/es6-iterators-generators-and-iterables/)
*   [The Basics Of ES6 Generators](http://davidwalsh.name/es6-generators)

### Unicode

ES6 完整支持Unicode编码。

```javascript
// same as ES5.1
"𠮷".length == 2

// new RegExp behaviour, opt-in ‘u’
"𠮷".match(/./u)[0].length == 2

// new form
"\u{20BB7}"=="𠮷"=="\uD842\uDFB7"

// new String ops
"𠮷".codePointAt(0) == 0x20BB7

// for-of iterates code points
for(var c of "𠮷") {
  console.log(c);
}
```

### Modules 模块

*   ES6中原生支持模块的组件定义。
*   从流行的js模块加载器(AMD, CommonJS)中规范组件的编码和定义方式。
*   通过预定义的默认加载器来定义运行时行为。
*   隐式异步模型(知道需要的模块可以用才执行具体代码)

```javascript
// lib/math.js
export function sum(x, y) {
  return x + y;
}
export var pi = 3.141593;
```

```javascript
// app.js
import * as math from "lib/math";
alert("2π = " + math.sum(math.pi, math.pi));
```

```javascript
// otherApp.js
import {sum, pi} from "lib/math";
alert("2π = " + sum(pi, pi));
```

`export default` 和 `export *` 的使用方式

```javascript
// lib/mathplusplus.js
export * from "lib/math";
export var e = 2.71828182846;
export default function(x) {
    return Math.exp(x);
}
```

```javascript
// app.js
import exp, {pi, e} from "lib/mathplusplus";
alert("2π = " + exp(pi, e));
```

### Module Loaders 模块加载器

*   动态加载(Dynamic Loading)
*   状态隔离(State Isolation)
*   全局域空间隔离(Global Namespace Isolation)
*   自定义钩子(Compilation Hooks)
*   嵌套虚拟化(Nested Virtualization)

默认的模块加载器可以被配置，也可以构建新的加载器来加载隔离的代码或者受限制的上下文。

```javascript
// Dynamic loading – ‘System’ is default loader
System.import('lib/math').then(function(m) {
  alert("2π = " + m.sum(m.pi, m.pi));
});

// Create execution sandboxes – new Loaders
var loader = new Loader({
  global: fixup(window) // replace ‘console.log’
});
loader.eval("console.log('hello world!');");

// Directly manipulate module cache
System.get('jquery');
System.set('jquery', Module({$: $})); // WARNING: not yet finalized
```

### Map + Set + WeakMap + WeakSet

对于常用算法来说高效的数据结构，而WeakMaps提供了纺织内存泄露的键值对数据结构，相比来说更安全。

```javascript
// Sets
var s = new Set();
s.add("hello").add("goodbye").add("hello");
s.size === 2;
s.has("hello") === true;

// Maps
var m = new Map();
m.set("hello", 42);
m.set(s, 34);
m.get(s) == 34;

// Weak Maps
var wm = new WeakMap();
wm.set(s, { extra: 42 });
wm.size === undefined

// Weak Sets
var ws = new WeakSet();
ws.add({ data: 42 });
// 因为添加的对象没有其他的引用，因此其并不会在这个weak set中存储。
```

### Proxies

Proxies 可以为对象增加监听器和处理函数

```javascript
// 监听普通对象
var target = {};
var handler = {
  get: function (receiver, name) {
    return `Hello, ${name}!`;
  }
};

var p = new Proxy(target, handler);
p.world === 'Hello, world!';
```

```javascript
// 监听函数对象
var target = function () { return 'I am the target'; };
var handler = {
  apply: function (receiver, ...args) {
    return 'I am the proxy';
  }
};

var p = new Proxy(target, handler);
p() === 'I am the proxy';
```

可以监听以下属性：

```javascript
var handler =
{
  get:...,
  set:...,
  has:...,
  deleteProperty:...,
  apply:...,
  construct:...,
  getOwnPropertyDescriptor:...,
  defineProperty:...,
  getPrototypeOf:...,
  setPrototypeOf:...,
  enumerate:...,
  ownKeys:...,
  preventExtensions:...,
  isExtensible:...
}
```

### Symbols

通过Symbols我们可以实现Object对象的访问控制。我们知道对象其实是键值对的集合，而键可以是字符串(ES5)或者Symbol。Symbol是新的基本类型。

Symbol 通过调用Symbol()函数产生，它接收一个可选的名字参数`name`(主要用来进行调试，并不作为定义的一部分)，该函数返回的symbol是唯一的。之后就可以用这个返回值做为对象的键了。Symbol还可以用来创建私有属性，外部无法直接访问由symbol做为键的属性值。

```javascript
(function() {

  // module scoped symbol
  var key = Symbol("key");

  function MyClass(privateData) {
    this[key] = privateData;
  }

  MyClass.prototype = {
    doStuff: function() {
      ... this[key] ...
    }
  };

})();

var c = new MyClass("hello")
c["key"] === undefined
```

### Subclassable Built-ins

在ES6中，内置的像 `Array` , `Date` 和 DOM 元素等可以用来作为基类。

被称为 `Ctor` 函数的对象构建经过两个阶段：

*   调用 `Ctor[@@create]` 来分配对象。
*   在新实例中唤起构造函数进行初始化。

```javascript
// Array 伪代码
class Array {
    constructor(...args) { /* ... */ }
    static [Symbol.create]() {
        // Install special [[DefineOwnProperty]]
        // to magically update 'length'
    }
}

// Array 基类的扩展
class MyArray extends Array {
    constructor(...args) { super(...args); }
}

// 使用new的两个阶段：
// 1) 调用@@create来分配对象
// 2） 在新实例中唤醒构造函数
var arr = new MyArray();
arr[1] = 12;
arr.length == 2
```

更多内容请参考 http://www.2ality.com/2013/03/subclassing-builtins-es6.html

### Math + Number + String + Object APIs

在ES6中增加了许多新的API，包括核心Math库，Array conversion helpers，用于对象拷贝的`Object.assign`等。

```javascript
Number.EPSILON
Number.isInteger(Infinity) // false
Number.isNaN("NaN") // false

Math.acosh(3) // 1.762747174039086
Math.hypot(3, 4) // 5
Math.imul(Math.pow(2, 32) - 1, Math.pow(2, 32) - 2) // 2

"abcde".contains("cd") // true
"abc".repeat(3) // "abcabcabc"

Array.from(document.querySelectorAll('*')) // Returns a real Array
Array.of(1, 2, 3) // Similar to new Array(...), but without special one-arg behavior
[0, 0, 0].fill(7, 1) // [0,7,7]
[1,2,3].findIndex(x => x == 2) // 1
["a", "b", "c"].entries() // iterator [0, "a"], [1,"b"], [2,"c"]
["a", "b", "c"].keys() // iterator 0, 1, 2
["a", "b", "c"].values() // iterator "a", "b", "c"

Object.assign(Point, { origin: new Point(0,0) })
```

### Binary and Octal Literals

ES6中为二级制(b)和八进制(o)增加了两个新的数字字面量格式。

```javascript
0b111110111 === 503 // true
0o767 === 503 // true
```

### Promises

Promises是用来处理异步编程的库，已经有很多第三方库对其进行了来实现。

```javascript
//创建promise
var promise = new Promise(function(resolve, reject) {
    // 进行一些异步或耗时操作
    if ( /*如果成功 */ ) {
        resolve("Stuff worked!");
    } else {
        reject(Error("It broke"));
    }
});
//绑定处理程序
promise.then(function(result) {
    //promise成功的话会执行这里
    console.log(result); // "Stuff worked!"
}, function(err) {
    //promise失败会执行这里
    console.log(err); // Error: "It broke"
});
```

更多Promises信息请访问:

*   https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
*   https://www.promisejs.org/
*   http://es6.ruanyifeng.com/#docs/promise

### Tail Calls

尾部的函数调用保证了堆栈不会无限增长，使得递归算法在面对无限制输入时变得安全可靠。

```javascript
function factorial(n, acc = 1) {
    'use strict';
    if (n <= 1) return acc;
    return factorial(n - 1, n * acc);
}

// 目前大部分的实现都会导致栈溢出
// 但是在ES6中对于任意输入都是安全的。
factorial(100000)
```

