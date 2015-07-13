title: "Javascript 学习笔记"
date: "2014-02-02"
excerpt: true
comments: true
tags:
    - Javascript
    - 笔记
---

##创建Object实例

1.  使用new操作符后跟Object构造函数

```javascript
var persion = new Object()
persion.name = "Eric"
persion.age = 27
```

2.  **对象字面量**表示法

```javascript
var persion = {
    name : "Eric",
    age: 27
}

```

<!-- more -->

3.  利用**对象字面量**表示法先创建一个Object对象，再赋值

```javascript
var persion = {}
persion.name = "Eric"
persion.age = 27
```

**Note**: 使用对象字面量表示法定义对象时，不会调用Object构造函数(Firefox除外)

---

##创建Array类型数组

1.  基本方式，使用Array构造函数

```javascript
var colors_empty = new Array()              #创建空数组
```

    当给Array()传递的参数为一个值的时候，如果传递的参数为数字，则根据传入的数字生成相应长度的数组；如果传入的是其它类型的参数，则生成包含该参数的长度为1的数组。

```javascript
var colors_with_20_length = new Array(20)   #创建长度为20的数组，每一项初始值为undefined
var colors_with_1_arg = new Array('Eric')   #创建长度为1的数组，colors_with_1_arg[0]为'Eric'
```

    使用Array构造函数时，new操作符可以省略：

```javascript
var colors = Array(3)
var names = Array('Eric')
```

2.  字面量表示法

```javascript
var colors = ['red','blue','green']         #创建包含3个字符串的数组
var names = []                              #创建一个空数组
var do_not_create_array_like_this = [1,2,]  #在不同的解释器下数组长度可能不同，容易产生异常错误，不要如此定义！
```

**Note**: 使用对象字面量表示法定义对象时，不会调用Array构造函数(Firefox除外)

---

##数组的一些操作

###转换方法

`toLocalString()` | `toString()` | `valueOf()` | `join()`

*  `toString()`和`valueOf()`的区别：`toString()`偏重于显示，默认情况下它会将数据类型转换成String类型；`valueOf`会返回数据类型原本所拥有的数据类型。例如如下代码可以做一些区分。

```javascript
var test_type_of_number = 123
console.log(typeof test_type_of_number)                 #String
console.log(typeof test_type_of_number.valueOf())       #number
console.log(typeof test_type_of_number.toString())      #String
```

*  在JavaScript高级程序设计(V2)中描述"调用数组的`toString()`和`valuOf()`方法会返回相同的值，我认为是错误的。实际执行过程中发现:

```javascript
var test_list = [1,2,3,'4']
console.log(test_list.valueOf())                        #[1,2,3,'4']
console.log(test_list.toString())                       #1,2,3,4
console.log(test_list.join())                           #1,2,3,4
console.log(test_list.join(','))                        #1,2,3,4
```

*  从这点上来看，Array中的`toString()`方法会调用数组中每一项数据的`toString()`方法来获取所对应的字符串再用`,`链接起来。而`join()`和`join(',')`的执行过程与`toString()`完全一样。

*  `toLocalString()`和`toString()`的不同之处在于，他会调用数组中每一个子项的`toLocalString()`方法，而不是`toString()`，所以在使用的时候需要注意区分。利用`toLocalString()`可以实现不同格式的格式化。

**Note**: 利用`alert(xxx)`来查看的时候他会自动调用对应的toString()方法来进行显示。因此`alert(list.valueOf())`实际执行的是`alert('list.valueOf().toString()')`

```javascript
var list = [1,2,3,4]
alert(list)                                             #1，2，3，4
alert(list.valueOf())                                   #1，2，3，4
alert(list.toString())                                  #1，2，3，4
```

###栈方法

`push()` | `pop()`

*   无难点，LIFO(Last-In-Frist-Out)栈结构的具体实现和操作方式

###队列方法

`push()` | `shift()` | `unshift()` | `pop()`

*   FIFO(First-In-Frist-Out)队列结构的具体实现和操作
*   `push()`和`shift()`配合使用可以实现队列结构的具体操作
*   'unshift()'和'pop()'配合使用可以实现逆序队列结构的具体操作

```javascript
var list = [1,2,3,4]
list.push(5)
console.log(list)                                       #[1,2,3,4,5]
console.log(list.shift())                               #1
console.log(list)                                       #[2,3,4,5]
list.unshift(6)
console.log(list)                                       #[6,2,3,4,5]
console.log(list.pop())                                 #5
console.log(list)                                       #[6,2,3,4]
```

###排序方法

`reverse()` | `sort()`

*   `reverse()`方法就是将数组逆序排序
*   `sort()`方法相对复杂很多。首先`sort()`会调用数组中每一项元素的`toString`方法将其转换成String类型，然后对字符串的首字母进行排序比较。因此5和'10'排序，'10'会排在5前面。
*   `sort(function(value1,value2){})`可以接收一个比较函数，来改变`sort()`的默认比较行为。

```javascript
var list = [1,2,3,11]
list.sort()                                             #[1,11,2,3]
list.sort(function(x,y){return x-y})                    #[1,2,3,11]
```

###操作方法

`concat()` | `slice()` | `splice()`

*   `concat()`可以合并数组

```javascript
var list = [1,2,3]                                      #[1,2,3]
var list_2 = list.concat()                              #[1,2,3]
var list_3 = list.concat(4,5,[6,7,8])                   #[1,2,3,4,5,6,7,8]
```
*   `slice()`切片方法接受1~2个参数。1个参数的表示返回从当前位置到末尾的新数组，类似于`list[index:]`; 2个参数表示返回从第一个参数到第二个参数（不包含第二个参数）的新数组，类似于`list[start,end]`。如果输入的参数为负数，表示从数组末尾往前数。

```javascript
var list = [0,1,2,3,4]
var new_list = list.slice(2)                            #[2,3,4]
new_list = list.slice(2,4)                              #[2,3]
new_list = list.slice(-2)                               #[3,4]
new_list = list.slice(-4,-2)                            #[1,2]
```

*   `splice()`是对数组的操作方法，可以实现数组的增删改查功能，其输入参数格式如下：`splice(起始位置，要删除的项数，要插入的项)`，其中起始位置表示操作的起始index，要删除的项目表示从其实位置要删除几条数据，要插入的项表示都有哪些数据要插入(可以插入多项)
    *   删除: `splice(起始位置, 删除的项数)`
    *   插入：`splice(起始位置, 0, 插入的数据)`
    *   替换：`splice(起始位置, 1, 替换的数据)`
*   测试代码如下：

```javascript
var list = [0,1,2,3,4]
list.splice(1,2)                                        #[0,3,4]
list.splice(1,0,1,2)                                    #[0,1,2,3,4]
list.splice(1,1,5,6)                                    #[0,5,6,2,3,4]
```

##Function

在JavaScript中，函数是Function类的具体实例。而且都与其它引用类型一样具有属性和方法。函数名实际上是指向函数对象的指针，函数可以作为参数参与到传参和返回值中。

###函数定义

```javascript
//函数声明式定义
function foo(num1,num2){
    return num1 + num2;
}
//函数表达式定义
var foo = function(num1,num2){
    return num1 + num2;
};
//使用Function构造函数定义
var foo = new Function("num1","num2","return num1 + num2")；
//实际上创建一个Function实例并不一定要赋值给具体的指针，可以直接执行
(function(x,y){return x+y})(1,2);
//之所以用圆括号把function(){}括起来是因为js解释器会将function解释为函数声明，而函数声明不能直接跟着(x,y)，我们需要将其转换为函数表达式。
//(1,2)表示要传递跟函数的参数。
//上面的例子也可以写成：
function foo(x,y){
    return x+y;
}(1,2);
//函数声明的方式无法定义匿名函数，因此如果想使用匿名函数，则必须用函数表达式的定义方式。
```

###函数的对象特性

因为函数是Function的实例，而函数名仅仅是该实例的一个引用地址。因此可以作为参数和返回值参与到函数的传参过程中。

```javascript
function call_some_function(some_function, some_argument) {
    return some_function(some_argument);
}
function add_10(num) {
    return num + 10;
}
console.log(call_some_function(add_10,20)); //30
```

###函数的内部属性

`arguments` | `this`

*   `arguments`对象中保存着传递给函数的参数
*   `arguments.length`返回传入参数的个数
*   **Note**: `length`属性表示函数定义时候默认接收的参数数量。`arguments.length`表示函数实际执行时接收的参数数量。

```javascript
function test_arguments() {
    if (arguments.length == 2) {
        console.log(arguments.length); 
        console.log(arguments); 
    } else {
        console.log(arguments.length); 
        console.log(arguments); 
        arguments.callee(4, 5);
    };
}(1, 2, 3)
/**
 3
{ '0': 1, '1': 2, '2': 3 }
2
{ '0': 4, '1': 5 }
 **/
```
*   `arguments.callee()`

主要用在递归函数中调用函数自身的情境中。js和别的语言不同在于函数名只是一个指针，可以随时变化，函数中利用函数名来调用自身属于高耦合，可能会出现问题，而`arguments.callee()`调用自身就会规避掉这个问题

```javascript
function factorial(num) {
    if (num <= 1) {
        return 1;
    } else {
        return num * factorial(num - 1);
    };
}
function callee_f(num) {
    if (num <= 1) {
        return 1;
    } else {
        return num * arguments.callee(num - 1);
    };
}
factorial(10); //运行正常
f = factorial;
factorial = null;
f(10); //error

callee_f(10); //运行正常
f = callee_f;
callee_f = null;
f(10); //运行正常
```

*   `this`主要用来帮助函数引用函数所处作用域中的对象。

```javascript
var color = 'red';
function syaColor() {
    console.log(this.color);
}
syaColor(); //red
var o = new Object();
o.color = 'blue';
o.sayColor = sayColor;
o.sayColor(); //blue
```

###call()和apply()

call()和apply()是每个函数都包含的自有方法。之前已经提到了函数是定义的对象，那么调用函数时候，函数中的`this`是对当前与下变量的调用。而如果想改变函数执行所在域空间，则可以使用call()和apply()来实现。

```javascript
color = 'red';
var o = {color: 'blue'};
function sayColor() {
    console.log(this.color);
}
sayColor(); //red
sayColor.call(this); //red
sayColor.call(o); //blue
```
app()和call()的作用是相同的，区别主要在于传入参数的不同。
>`call(this,para1,prar2,prar3)` 第一个参数是函数要执行的作用域，后面的参数是函数的输入参数，有多少个依次写多少个。

>`apply(this,[para1,para2,prara3])`第一个参数也是函数要执行的作用域，后面是一个Array的数组对象。

使用`call()`/`apply()`来扩充作用域最大的好处是对象和方法的解耦。

##内置对象

`Global`对象可以理解成最外层的对象，所有的对象，以及不属于其它对象的属性和方法都被包含在Global对象中。

*   `isNaN(x)` 用来检查参数x是否为数字。如果为数字返回false，否则返回true
*   `isFinite(x)` 用来检查参数x是否为无穷大/小，如果是无穷大/小，则返回true
*   `parseInt(x)` 用来解析字符串并返回整数
*   `parseFloat(x)` 用来解析字符串并返回浮点数
*   `encodeURI()`和`encodeURIComponent()`会对字符串进行特殊的UTF-8编码，规避一些特殊字符来让浏览器能够读懂。他俩的区别主要在于`encodeURI()`不会对本身属于URI的特殊字符进行编码，而`encodeURIComponent()`会对其发现的所有非标准字符进行编码。

```javascript
var uri = "http://www.wrox.com/illegal value.htm#start";
//http://www.wrox.com/illegal%20value.htm#start
console.log(encodeURI(uri))
//http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.htm%23start
console.log(encodeURIComponent(uri))
```
*   对应的解码函数为`decodeURI()`和`decodeURIComponent()`
*   `eval(script)` 用来将script的内容在解释器中执行并返回对应的结果。非常强大！

**Note**:在浏览器中，windows对象封装了Global对象，并承担了很多额外的任务和功能。

`Math`对象为另一个内置对象。为JavaScript提供了数学计算功能。

##函数闭包

先看一段代码

```javascript
function f(){
    var n = 100;
}
f()
```

变量n定义在f()中，属于作用于f的内部变量，当f()执行结束后变量的生存周期就结束了，所开辟的内存空间也将会被垃圾回收。

再看另一段代码

```javascript
function f1(){
    var n = 100;
    return function f2(){console.log(n);}
}
var result = f1()
console.log(result()) //100
```

我们在函数f1()外面定义了变量resul来接受f1()执行以后的返回值，f1()的返回值是一个匿名函数，该函数调用了f1()内部定义的变量n。

我们发现虽然f1()执行完成了，但是执行`result()`会输出`100`，说明变量n并没有失效，他可以在f1()作用域外被result函数访问。

我们把这种函数中引用函数作用域以外变量的特性叫做--**函数的闭包**

函数闭包可以扩大引用变量的生存周期和使用范围，但也有可能造成内存泄露。


## new关键字

为了弄明白new, 对象和函数之间的关系，首先看一下以下代码：

```javascript
Function instanceof Object  #true
Object instanceof Function  #true
```

这个问题先放一下，再来几段代码

```javascript
var A = function(){this.p = 1;}
var a1 = A()        #undefined
var a2 = new A()    #{p:1}
```
我们可以看到，`A()`表示执行函数A，赋值A函数执行后的返回值给a1,因为函数A没有`return`，因此默认返回`undefined`，所以a1为`undefined`。

那么`new A()`为什么会返回`{p:1}`？实际上行`new A()`会调用`A()`的属性`constructor`，其为一个函数，该函数执行以下步骤：

*   创建新的对象x
*   判断`A.prototype`
    *   如果是`object`，则执行`x.__proto__ = A.prototype`
    *   如果不是`object`，则执行`x.__proto__ = Object.prototype`
*   执行`result = A.call(x)`
    *   如果`result`是`object`，则返回result
    *   如果`result`不是`object`，则返回x

具体`call()`函数的作用会在后面说明。

简单用示例来进一步说明

```javascript
var A = function(){this.p =1;}
var a = new A()
```
等价于
```javascript
var A = function(){this.p = 1;}
function createObject(O){
    var x = new Object();
    x.__proto__ = O.prototype 
    var result = O.call(x)
    if (typeof(result) == "object"){
      return result;
    }
    return x;
}
var a = createObject(A);
```

根据以上`new`创建对象的流程来看几个例子

```javascript
var A = function(){this.p = 1;}
A.prototype = {m:3}
var a = new A()         #{p:1}
a.m                     #3
A.prototype = 3
a = new A()             #{P:1}
a.__proto__             #{}

var B = function(){this.p = 1; return {q:2}}
B.prototype = {m:3}
var b = new B()         #{q:2}
b.m                     #undefined

var C = function(){this.p = 1; return 2}
C.prototype = {m:3}
var c = new C()         #{p:1}
c.m                     #3
```

以上我们知道了通过`new`来创建一个函数对象的过程。

>那么函数本身到底是什么呢？

函数实际上是对象，每个函数都是`Function`类型的实例。而`Object`、`Functioon`，甚至包括`Array`在内的类型实际上都是具体有特殊`constructor`的对象。因此`Function instanceof Object`返回`true`

又因为`Object`本身实际上是一个`[Function: Object]`即返回Object的函数，因此`Object instanceof Function`便返回true了。



##局部变量和全局变量

```javascript
function test(){
    var message = "hi"; //局部变量
}
test();
alert(message); //错误
```

```javascriot
function test(){
    message = "hi"; //全局变量
}
test();
alert(message); //hi
```

```
var color = "blue";
function changeColor(){
    var anotherColor = "red";
    function swapColors() {
        var tempColor = anotherColor;
        anotherColor = color;
        color = tempColor;
        // 这里可以访问color, anotherColor 和 tempColor
    }
    // 这里可以访问 color 和 anotherColor，但不能访问 tempColor
}
// 这里只能访问color
```

以上代码共涉及 3 个执行环境:全局环境、 changeColor() 的局部环境和 swapColors() 的局部环境。全局环境中有一个变量 color 和一个函数 changeColor() 。 changeColor() 的局部环境中有一个名为 anotherColor 的变量和一个名为 swapColors()的函数,但它也可以访问全局环境中的变量 color 。 swapColors() 的局部环境中有一个变量 tempColor ,该变量只能在这个环境中访问到。无论全局环境还是 changeColor() 的局部环境都无权访问 tempColor 。然而,在 swapColors() 内部则可以访问其他两个环境中的所有变量,因为那两个环境是它的父执行环境。

```javascript
if(true) {
    var color = "blue";
}
alert(color);
// "blue"
```

这里是在一个 if 语句中定义了变量 color 。如果是在 C、C++或 Java 中, color 会在 if 语句执行完毕后被销毁。但在 JavaScript 中, if语句中的变量声明会将变量添加到当前的执行环境(在这里是全局环境)中。在使用 for 语句时尤其要牢记这一差异,例如:

```javascript
for(var i=0; i<10, i++){
    doSomething(i);
}
alert(i);
//10
```

对于有块级作用域的语言来说, for 语句初始化变量的表达式所定义的变量,只会存在于循环的环
境之中。而对于 JavaScript 来说,由 for 语句创建的变量 i 即使在 for 循环执行结束后,也依旧会存在于循环外部的执行环境中。

### 5种简单数据类型(也称为基本数据类型): Undefined、Null、Boolean、Number和 String 。还有 1 种复杂数据类型—— Object

   * undefined ——如果这个值未定义;
   * boolean ——如果这个值是布尔值;
   * string ——如果这个值是字符串;
   * number ——如果这个值是数值;
   * object ——如果这个值是对象或 null ;
   * function ——如果这个值是函数。

### Object 的每个实例都具有下列属性和方法。

   * onstructor :保存着用于创建当前对象的函数。对于前面的例子而言,构造函数(constructor)
就是 Object() 。
   * hasOwnProperty(propertyName) :用于检查给定的属性在当前对象实例中(而不是在实例
的原型中)是否存在。其中,作为参数的属性名( propertyName )必须以字符串形式指定(例
。
如: o.hasOwnProperty("name") )
   * isPrototypeOf(object):用于检查传入的对象是否是传入对象的原型(第 5 章将讨论原
型)。
   * propertyIsEnumerable(propertyName) :用于检查给定的属性是否能够使用 for-in 语句
(本章后面将会讨论)来枚举。与 hasOwnProperty() 方法一样,作为参数的属性名必须以字符
串形式指定。
   * toLocaleString() :返回对象的字符串表示,该字符串与执行环境的地区对应。
   * toString() :返回对象的字符串表示。
   * valueOf() :返回对象的字符串、数值或布尔值表示。通常与 toString() 方法的返回值
相同。

### instanceof 操作

   * alert(person instanceof Object);// 变量 person 是 Object 吗?
   * alert(colors instanceof Array);// 变量 colors 是 Array 吗?
   * alert(pattern instanceof RegExp);// 变量 pattern 是 RegExp 吗?

### 栈方法

```javascript
var colors = ["red", "blue"];
colors.push("brown"); // 添加另一项
colors[3] = "black"; // 添加一项alert
(colors.length); // 4 
var item = colors.pop(); // 取得最后一项
alert(item); //"black"
```

### 队列方法

```javascript
var colors = new Array();//创建一个数组
var count = colors.unshift("red", "green");//推入两项
alert(count);//2
count = colors.unshift("black");//推入另一项
alert(count);//3
var item = colors.pop(); //取得最后一项
alert(item);//"green"
alert(colors.length); //2
```

### 自定义排序

```javascript
function compare(value1, value2) {
    if (value1 < value2) {
        return 1;
    } else if (value1 > value2) {
        return 2;
    } else {
        return 0;
    }
}
function newcompare(value1, value2) {
    return value2 - value1;
}
var values = [0, 1, 5, 10, 15]
value.sort(compare)
alert(values)
// 15, 10, 5, 1, 0
```

### 数组操作splice()

splice() 方法始终都会返回一个数组,该数组中包含从原始数组中删除的项(如果没有删除任何
项,则返回一个空数组)。下面的代码展示了上述 3 种使用 splice() 方法的方式。

```javascript
var colors = ["red", "green", "blue"]; 
var removed = colors.splice(0,1);// 删除第一项
alert(colors);// green,blue
alert(removed);// red,返回的数组中只包含一项 
removed = colors.splice(1, 0, "yellow", "orange");// 从位置 1 开始插入两项
alert(colors);// green,yellow,orange,blue
alert(removed);// 返回的是一个空数组 
removed = colors.splice(1, 1, "red", "purple");// 插入两项,删除一项
alert(colors);// green,red,purple,orange,blue
alert(removed);// yellow,返回的数组中只包含一项
```

### trim()方法

这个方法会创建一个字符串的副本,删除前置及后缀的所有空格,然后返回结果

```javascript
var stringValue = " hello world "; 
var trimmedStringValue = stringValue.trim(); 
alert(stringValue);//" hello world "
alert(trimmedStringValue);//"hello world"
```

### 字符串大小写转换方法

```javascript
var stringValue = "hello world"; 
alert(stringValue.toLocaleUpperCase());//"HELLO WORLD"
alert(stringValue.toUpperCase());//"HELLO WORLD"
alert(stringValue.toLocaleLowerCase());//"hello world"
alert(stringValue.toLowerCase());//"hello world"
```

toLocaleLowerCase() 和 toLocaleUpperCase()方法则是针对特定地区的实现,一般来说,在不知道自己的代码将在哪种语言环境中运行的情况下,还是使用针对地区的方法更稳妥一些。

