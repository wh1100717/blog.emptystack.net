title: "ECMAScript 5 学习笔记"
date: "2015-04-02"
excerpt: true
comments: true
tags:
    - ECMAScript
    - 笔记
---

浏览器兼容性：Opera 11.60+、Internet Explorer 9+、Firefox 4+、Safari 5.1+、Chrome 13+

> IE9不支持严谨模式，但IE10是支持的

为了保证js代码得到通用性的支持，可以在html中增加如下代码：

```html
<!--[if lt IE 9]>
<script src="https://cdnjs.cloudflare.com/ajax/libs/es5-shim/4.0.5/es5-shim.js"></script>
<![endif]-->
```

> es5-shim提供了ES5的部分实现，其中有一部分需要依赖底层engine，为了保证在地浏览器中js不报错，对其中该部分的API做了封装，但是并不保证一定能正确执行，需要额外引用es5-sham。简单来说，如果需要考虑浏览器兼容性的问题，则不要使用Object在ES5上进行的扩展。具体请参考如下文档 https://github.com/es-shims/es5-shim 。

## Array

### Array.isArray(arg)

isArray 函数需要一个参数 arg，如果参数是个对象并且 class 内部属性是 "Array", 返回布尔值 true；否则它返回 false。采用如下步骤：

*   如果 Type(arg) 不是 Object, 返回 false。
*   如果 arg 的 [[Class]] 内部属性值是 "Array", 则返回 true。
*   返回 false.

```javascript
Array.isArray([1,2,3])  //true
Array.isArray({a:1})    //false
```

### Array.prototype.every(callbackfn [, thisArg])

检查数组的每一个元素是否符合callbackfn的要求。只有遍历数组中的每一个元素都返回true时，最终才会返回true，否则返回false。

```javascript
var arr1 = [1,2,3,4,5]
var arr2 = [1,2,'3',4,5]
var isNumber = function(item, index){
    console.log(typeof item, index);
    return typeof item === 'number';
}
//当数组按照索引全部遍历完并且每次callbackfn都返回true，则最终every函数返回true
arr1.every(isNumber)
/*
number 0
number 1
number 2
number 3
number 4
true
*/
//当callbackfn返回false，则every函数直接返回false
arr2.every(isNumber)
/*
number 0
number 1
string 2
false
*/
var isNumberArg = function(item, index){
    console.log(this.x, index);
    return typeof item === 'number'
}
//我们可以给every函数添加指定域，可以通过this.attr来引用域中的属性。
arr2.every(isNumberArg, {x:1})
/*
1 0
1 1
1 2
false
*/
```

### Array.prototype.some(callbackfn [, thisArg])

与Array.prototype.every相反，只要有任何一个元素返回true，则返回true，否则返回false。
thisArg与Array.prototype.every中使用方法相同。

```javascript
var arr = [1,2,3,4,5]
arr.some(function(item, index){
    return item > 4;
})
```

### Array.prototype.filter(callbackfn [, thisArg])

过滤器，返回满足callbackfn的item项(即callbackfn为true的项)。 thisArg与Array.prototype.every中使用方法相同。

```javascript
var arr = [1,2,'3',4,5]
var isNumber = function(item, index){
    console.log(typeof item, index);
    return typeof item === 'number';
}
arr.filter(isNumber)
/*
number 0
number 1
string 2
number 3
number 4
[1, 2, 4, 5]
*/
```

### Array.prototype.forEach(callbackfn [, thisArg])

使用callbackfn来遍历数组，返回undefined, thisArg与Array.prototype.every中使用方法相同。

```javascript
var arr = [1,2,3,4,5]
arr.forEach(function(item, index, array){
    console.log(item, index);
})
/*
1 0
2 1
3 2
4 3
5 4
*/
```

### Array.prototype.indexOf(searchElement [, fromIndex])

按照数组升序返回数组中元素与searchElement严格相等的元素, fromIndex表示开始遍历查询的元素下标。

```javascript
var arr = ["hello", "world", "hello", "man", {a: 1}]
arr.indexOf("hello")    //0
arr.indexOf("hello", 1) //2
arr.indexOf({a:1})      //-1  因为{a: 1}为两个对象，并不严格相等，因此不会返回其对应的index
```

### Array.prototype.lastIndexOf(searchElement [, fromIndex])

与Array.prototype.indexOf，不过期按照数组降序进行匹配。fromIndex表示开始遍历查询的元素下标。

```javascript
var arr = ["hello", "world", "hello", "man", {a: 1}]
arr.lastIndexOf("hello")    //2
arr.lastIndexOf("hello", 1) //0
```

### Array.prototype.map(callbackfn [, thisArg])

对数组内的元素按照callbackfn进行更改并得到新的数组。thisArg与Array.prototype.every中使用方法相同。

```javascript
var arr = [1,2,3,4,5]
arr2 = arr.map(function(item, index){
    return item * item
})
console.log(arr2)   //[1, 4, 9, 16, 25]
```

### Array.prototype.reduce(callbackfn [, initialValue])

callbackfn接受4个参数：

*   previousValue为之前值(如果不指定initialValue，则再第一次迭代previousValue为下标为0的元素值，currentValue为数组下标为1的元素，如果数组只有一个元素，则直接返回该元素)
*   currentValue为当前值
*   currentIndex为当前索引值
*   array为数组本身。

initialValue可以指定初始值。

```javascript
var arr = ["hello", "world", "aaa", "bbb"]
var reduce_str = arr.reduce(function(previousValue, currentValue, currentIndex, array){
    return previousValue + "[" + currentValue + "]"
})
console.log(reduce_str)     //hello[world][aaa][bbb]
var reduce_str2 = arr.reduce(function(previousValue, currentValue, currentIndex, array){
    return previousValue + "[" + currentValue + "]"
}, "[Init Value]")
console.log(reduce_str2)    // [Init Value][hello][world][aaa][bbb]
```

### Array.prototype.reduceRight(callbackfn [, initialValue])

与reduce相比，用法类似。实现上差异在于reduceRight是从数组的末尾开始实现。

```javascript
var arr = ["hello", "world", "aaa", "bbb"]
var reduce_str = arr.reduceRight(function(previousValue, currentValue, currentIndex, array){
    return previousValue + "[" + currentValue + "]"
})
console.log(reduce_str)     // bbb[aaa][world][hello]
var reduce_str2 = arr.reduceRight(function(previousValue, currentValue, currentIndex, array){
    return previousValue + "[" + currentValue + "]"
}, "[Init Value]")
console.log(reduce_str2)    // [Init Value][bbb][aaa][world][hello]
```

## Date

### Date.now()

now 函数返回一个数字值，它表示调用 now 时的 UTC 日期时间的时间值。等价于(new Date()).getTime()

```
console.log("Current Time is: ", Date.now())
```

### Date.prototype.toJSON(key)

此函数为 JSON.stringify 提供 Date 对象的一个字符串表示。

```javascript
var now = new Date(1421762769200)
now.toJSON()    //"2015-01-20T14:06:09.200Z"
```

### Date.parse(string)

将字符串转为UTC时间值

```
Date.parse("2015-01-20T14:06:09.200Z")  //1421762769200
```

### Date.prototype.toISOString()

此函数返回一个代表 --this Date 对象表示的时间的实例 -- 的字符串。字符串的格式是 15.9.1.15 定义的日期时间字符串格式。字符串中包含所有的字段。字符串表示的时区总是 UTC，用后缀 Z 标记。如果 this 对象的时间值不是有限的数字值，抛出一个 RangeError 异常。

```javascript
var now = new Date(1421762769200)
now.toISOString()   //"2015-01-20T14:06:09.200Z"
```

## Number && String && Function

### parseInt(string, radix)

parseInt 函数根据指定的参数 radix，和 string 参数的内容解释结果来决定，产生一个整数值。
radix表示进制，范围在2和36之间，默认为10进制。

```javascript
parseInt("1234")        //1234
parseInt("10101", 2)    //21
```

### Number.prototype.toFixed(fractionDigits)

Number四舍五入，fractionDigits表示保留的位数。fractionDigits范围为0到20

```javascript
(123.456).toFixed()    //123
(123.456).toFixed(1)    //123.5
(123.456).toFixed(2)    //123.46
```

### String.prototype.split(separator, limit)

将字符串按照separator进行拆分，并返回对应的数组。其中separator可以为字符串或者正则表达式，limit限制返回数组的总长度。

```javascript
"hello,world,aaa,bbb".split(",")    //["hello", "world", "aaa", "bbb"]
"hello,world,aaa,bbb".split(",",2)  //["hello", "world"]
"a\nb\r\nc\rd".split(/\r?\n/)       //["a", "b", "cd"]
```

### String.prototype.trim()

返回去除字符串头尾空格和换行符的字符串。

```javascript
"  \n   t\ne\ts t   \n  \t".trim()
/*
"t
e   s t"
*/
```

### String.prototype.replace(searchValue, replaceValue)

将字符串按照sarchValue进行匹配，并按照repalceValue进行替换。searchValue可以为字符串或正则表达式，repalceValue可以为字符串或函数。

```javascript
var name = 'aaa bbb ccc';
var uw = name.replace(/\b\w+\b/g, function(word){
    return word.substring(0,1).toUpperCase()+word.substring(1);}
);
console.log(uw);    // Aaa Bbb Ccc
```

### Function.prototype.bind()

bind()方法会创建一个新函数,称为绑定函数.

当调用这个绑定函数时,绑定函数会以创建它时传入 bind()方法的第一个参数作为 this,传入 bind() 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数.

```javascript
var getX = function(m, n){return this.x + m + n}
var module = {
    x: 1
}
var boundGetX = getX.bind(module, 2, 3)
boundGetX() // 6
```

## Object

### Object.keys(obj)

以数组的形式返回Obj对象的所有key

```javascript
var obj = {a:1, b:2, c:3}
Object.keys(obj)    //["a", "b", "c"]
```

>注：Object.kyes()之后的的API在不支持的浏览器中会产生错误，引用es5-shim.js并不支持以下API，如果需要保证至少在低版本浏览器中不报错误，需要引用es5-sham.js，其并不能保证js会正确执行，但会禁止其对外暴露错误，请慎重使用。

### Object.create(O[, Properties])

以指定的原型创建对象，并且可以（可选）设置对象的属性。

Properties包含 value 特性，以及 writable、enumerable 和 configurable 特性。如果未指定最后三个特性，则它们默认为 false。

返回一个具有指定的内部原型且包含指定的属性（如果有）的新对象。# 数据大学 - ECMAScript 5 详解

标签（空格分隔）： 数据大学 ES5

---

浏览器兼容性：Opera 11.60+、Internet Explorer 9+、Firefox 4+、Safari 5.1+、Chrome 13+

> IE9不支持严谨模式，但IE10是支持的

为了保证js代码得到通用性的支持，可以在html中增加如下代码：

```html
<!--[if lt IE 9]>
<script src="https://cdnjs.cloudflare.com/ajax/libs/es5-shim/4.0.5/es5-shim.js"></script>
<![endif]-->
```

> es5-shim提供了ES5的部分实现，其中有一部分需要依赖底层engine，为了保证在地浏览器中js不报错，对其中该部分的API做了封装，但是并不保证一定能正确执行，需要额外引用es5-sham。简单来说，如果需要考虑浏览器兼容性的问题，则不要使用Object在ES5上进行的扩展。具体请参考如下文档 https://github.com/es-shims/es5-shim 。

## Array

### Array.isArray(arg)

isArray 函数需要一个参数 arg，如果参数是个对象并且 class 内部属性是 "Array", 返回布尔值 true；否则它返回 false。采用如下步骤：

*   如果 Type(arg) 不是 Object, 返回 false。
*   如果 arg 的 [[Class]] 内部属性值是 "Array", 则返回 true。
*   返回 false.

```javascript
Array.isArray([1,2,3])  //true
Array.isArray({a:1})    //false
```

### Array.prototype.every(callbackfn [, thisArg])

检查数组的每一个元素是否符合callbackfn的要求。只有遍历数组中的每一个元素都返回true时，最终才会返回true，否则返回false。

```javascript
var arr1 = [1,2,3,4,5]
var arr2 = [1,2,'3',4,5]
var isNumber = function(item, index){
    console.log(typeof item, index);
    return typeof item === 'number';
}
//当数组按照索引全部遍历完并且每次callbackfn都返回true，则最终every函数返回true
arr1.every(isNumber)
/*
number 0
number 1
number 2
number 3
number 4
true
*/
//当callbackfn返回false，则every函数直接返回false
arr2.every(isNumber)
/*
number 0
number 1
string 2
false
*/
var isNumberArg = function(item, index){
    console.log(this.x, index);
    return typeof item === 'number'
}
//我们可以给every函数添加指定域，可以通过this.attr来引用域中的属性。
arr2.every(isNumberArg, {x:1})
/*
1 0
1 1
1 2
false
*/
```

### Array.prototype.some(callbackfn [, thisArg])

与Array.prototype.every相反，只要有任何一个元素返回true，则返回true，否则返回false。
thisArg与Array.prototype.every中使用方法相同。

```javascript
var arr = [1,2,3,4,5]
arr.some(function(item, index){
    return item > 4;
})
```

### Array.prototype.filter(callbackfn [, thisArg])

过滤器，返回满足callbackfn的item项(即callbackfn为true的项)。 thisArg与Array.prototype.every中使用方法相同。

```javascript
var arr = [1,2,'3',4,5]
var isNumber = function(item, index){
    console.log(typeof item, index);
    return typeof item === 'number';
}
arr.filter(isNumber)
/*
number 0
number 1
string 2
number 3
number 4
[1, 2, 4, 5]
*/
```

### Array.prototype.forEach(callbackfn [, thisArg])

使用callbackfn来遍历数组，返回undefined, thisArg与Array.prototype.every中使用方法相同。

```javascript
var arr = [1,2,3,4,5]
arr.forEach(function(item, index, array){
    console.log(item, index);
})
/*
1 0
2 1
3 2
4 3
5 4
*/
```

### Array.prototype.indexOf(searchElement [, fromIndex])

按照数组升序返回数组中元素与searchElement严格相等的元素, fromIndex表示开始遍历查询的元素下标。

```javascript
var arr = ["hello", "world", "hello", "man", {a: 1}]
arr.indexOf("hello")    //0
arr.indexOf("hello", 1) //2
arr.indexOf({a:1})      //-1  因为{a: 1}为两个对象，并不严格相等，因此不会返回其对应的index
```

### Array.prototype.lastIndexOf(searchElement [, fromIndex])

与Array.prototype.indexOf，不过期按照数组降序进行匹配。fromIndex表示开始遍历查询的元素下标。

```javascript
var arr = ["hello", "world", "hello", "man", {a: 1}]
arr.lastIndexOf("hello")    //2
arr.lastIndexOf("hello", 1) //0
```

### Array.prototype.map(callbackfn [, thisArg])

对数组内的元素按照callbackfn进行更改并得到新的数组。thisArg与Array.prototype.every中使用方法相同。

```javascript
var arr = [1,2,3,4,5]
arr2 = arr.map(function(item, index){
    return item * item
})
console.log(arr2)   //[1, 4, 9, 16, 25]
```

### Array.prototype.reduce(callbackfn [, initialValue])

callbackfn接受4个参数：

*   previousValue为之前值(如果不指定initialValue，则再第一次迭代previousValue为下标为0的元素值，currentValue为数组下标为1的元素，如果数组只有一个元素，则直接返回该元素)
*   currentValue为当前值
*   currentIndex为当前索引值
*   array为数组本身。

initialValue可以指定初始值。

```javascript
var arr = ["hello", "world", "aaa", "bbb"]
var reduce_str = arr.reduce(function(previousValue, currentValue, currentIndex, array){
    return previousValue + "[" + currentValue + "]"
})
console.log(reduce_str)     //hello[world][aaa][bbb]
var reduce_str2 = arr.reduce(function(previousValue, currentValue, currentIndex, array){
    return previousValue + "[" + currentValue + "]"
}, "[Init Value]")
console.log(reduce_str2)    // [Init Value][hello][world][aaa][bbb]
```

### Array.prototype.reduceRight(callbackfn [, initialValue])

与reduce相比，用法类似。实现上差异在于reduceRight是从数组的末尾开始实现。

```javascript
var arr = ["hello", "world", "aaa", "bbb"]
var reduce_str = arr.reduceRight(function(previousValue, currentValue, currentIndex, array){
    return previousValue + "[" + currentValue + "]"
})
console.log(reduce_str)     // bbb[aaa][world][hello]
var reduce_str2 = arr.reduceRight(function(previousValue, currentValue, currentIndex, array){
    return previousValue + "[" + currentValue + "]"
}, "[Init Value]")
console.log(reduce_str2)    // [Init Value][bbb][aaa][world][hello]
```

## Date

### Date.now()

now 函数返回一个数字值，它表示调用 now 时的 UTC 日期时间的时间值。等价于(new Date()).getTime()

```
console.log("Current Time is: ", Date.now())
```

### Date.prototype.toJSON(key)

此函数为 JSON.stringify 提供 Date 对象的一个字符串表示。

```javascript
var now = new Date(1421762769200)
now.toJSON()    //"2015-01-20T14:06:09.200Z"
```

### Date.parse(string)

将字符串转为UTC时间值

```
Date.parse("2015-01-20T14:06:09.200Z")  //1421762769200
```

### Date.prototype.toISOString()

此函数返回一个代表 --this Date 对象表示的时间的实例 -- 的字符串。字符串的格式是 15.9.1.15 定义的日期时间字符串格式。字符串中包含所有的字段。字符串表示的时区总是 UTC，用后缀 Z 标记。如果 this 对象的时间值不是有限的数字值，抛出一个 RangeError 异常。

```javascript
var now = new Date(1421762769200)
now.toISOString()   //"2015-01-20T14:06:09.200Z"
```

## Number && String && Function

### parseInt(string, radix)

parseInt 函数根据指定的参数 radix，和 string 参数的内容解释结果来决定，产生一个整数值。
radix表示进制，范围在2和36之间，默认为10进制。

```javascript
parseInt("1234")        //1234
parseInt("10101", 2)    //21
```

### Number.prototype.toFixed(fractionDigits)

Number四舍五入，fractionDigits表示保留的位数。fractionDigits范围为0到20

```javascript
(123.456).toFixed()    //123
(123.456).toFixed(1)    //123.5
(123.456).toFixed(2)    //123.46
```

### String.prototype.split(separator, limit)

将字符串按照separator进行拆分，并返回对应的数组。其中separator可以为字符串或者正则表达式，limit限制返回数组的总长度。

```javascript
"hello,world,aaa,bbb".split(",")    //["hello", "world", "aaa", "bbb"]
"hello,world,aaa,bbb".split(",",2)  //["hello", "world"]
"a\nb\r\nc\rd".split(/\r?\n/)       //["a", "b", "cd"]
```

### String.prototype.trim()

返回去除字符串头尾空格和换行符的字符串。

```javascript
"  \n   t\ne\ts t   \n  \t".trim()
/*
"t
e   s t"
*/
```

### String.prototype.replace(searchValue, replaceValue)

将字符串按照sarchValue进行匹配，并按照repalceValue进行替换。searchValue可以为字符串或正则表达式，repalceValue可以为字符串或函数。

```javascript
var name = 'aaa bbb ccc';
var uw = name.replace(/\b\w+\b/g, function(word){
    return word.substring(0,1).toUpperCase()+word.substring(1);}
);
console.log(uw);    // Aaa Bbb Ccc
```

### Function.prototype.bind()

bind()方法会创建一个新函数,称为绑定函数.

当调用这个绑定函数时,绑定函数会以创建它时传入 bind()方法的第一个参数作为 this,传入 bind() 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数.

```javascript
var getX = function(m, n){return this.x + m + n}
var module = {
    x: 1
}
var boundGetX = getX.bind(module, 2, 3)
boundGetX() // 6
```

## Object

### Object.keys(obj)

以数组的形式返回Obj对象的所有key

```javascript
var obj = {a:1, b:2, c:3}
Object.keys(obj)    //["a", "b", "c"]
```

>注：Object.kyes()之后的的API在不支持的浏览器中会产生错误，引用es5-shim.js并不支持以下API，如果需要保证至少在低版本浏览器中不报错误，需要引用es5-sham.js，其并不能保证js会正确执行，但会禁止其对外暴露错误，请慎重使用。

### Object.create(O[, Properties])

以指定的原型创建对象，并且可以（可选）设置对象的属性。

Properties包含 value 特性，以及 writable、enumerable 和 configurable 特性。如果未指定最后三个特性，则它们默认为 false。

返回一个具有指定的内部原型且包含指定的属性（如果有）的新对象。

```javascript
var obj = Object.create({a:1},{
    x: {
        value: 1,
        writable: true,
        enumerable: true,
        configurable: true
    }
})
console.log(obj)            //{x: 1} #注：Chrome控制台会将对象的__proto__中的键值进行输出，而nodejs不会。
console.log(obj.__proto__)  //{a:1}
```

### Object.getPrototypeOf(obj)

返回对象的原型

```javascript
var A = function(){this.x = 1}
A.prototype.y = 2
var a = new A()
console.log(Object.getPrototypeOf(a))   //{y:2}
```

### Object.getOwnPropertyNames(obj)

返回对象自己的属性的名称。一个对象的自己的属性是指直接对该对象定义的属性，而不是从该对象的原型继承的属性。 对象的属性包括字段（对象）和函数。

```javascript
var A = function(){this.x = 1}
A.prototype.y = 2
var a = new A()
console.log(Object.getOwnPropertyNames(a))   //['x']
```

### Object.seal(obj)

阻止修改现有属性的特性(将每个属性的 configurable 设置为 false)，并阻止添加新属性。

```javascript
var obj = {a: 1, b: 2}
Object.seal(obj)
obj.c = 3
obj.a = 0
console.log(obj)    //{a: 0, b: 2}
console.log(obj.c)  //undefined
```

### Object.freeze(obj)

阻止修改现有属性的特性和值(将每个属性的 configurable 和 writable 设置为 false)，并阻止添加新属性。

```javascript
var obj = {a: 1, b: 2}
Object.freeze(obj)
obj.c = 3
obj.a = 0
console.log(obj)    //{a: 1, b: 2}
console.log(obj.c)  //undefined
```

### Object.preventExtensions(obj)

阻止向对象添加新属性。

```javascript
var obj = {a: 1, b: 2}
Object.preventExtensions(obj)
obj.c = 3
obj.a = 0
console.log(obj)    //{a: 0, b: 2}
console.log(obj.c)  //undefined
```

### Object.isSealed(obj)

如果无法在对象中修改现有属性的特性，且无法向对象添加新属性，则返回 true。

### Object.isFrozen(obj)

如果无法在对象中修改现有属性的特性和值，且无法向对象添加新属性，则返回 true。

### Object.isExtensible(obj)

返回一个值，该值指示是否可向对象添加新属性。

### Object.getOwnPropertyDescriptor(obj, propertyname)

获取指定对象自己的属性描述符。自己的属性描述符是直接在对象上定义的描述符，而不是从对象的原型继承的描述符。

```javascript
obj = {a: 1}
var descriptor = Object.getOwnPropertyDescriptor(obj, "a")
console.log(descriptor) //{ value: 1, writable: true, enumerable: true, configurable: true }
```

### Object.defineProperty(obj, propertyname, descriptor)

将属性添加到对象或修改现有属性的特性。

*   obj为要添加属性的对象。
*   propertyname为包含属性名称的字符串。
*   descriptor为属性的描述符。 它可以针对数据属性或访问器属性。

```javascript
var obj = {}
Object.defineProperty(obj, "newDataProperty", {
    value: 101,
    writable: true,
    enumerable: true,
    configurable: true
})
console.log(obj.newDataProperty)    //101
```

### Object.defineProperties(obj, descriptors)

将一个或多个属性添加到对象，并/或修改现有属性的特性。

descriptors包含一个或多个描述符对象的 JavaScript 对象。每个描述符对象描述一个数据属性或访问器属性。

```javascript
var obj = {}
Object.defineProperties(obj, {
    newDataProperty: {
        value: 101,
        writable: true,
        enumerable: true,
        configurable: true
    },
    newAccessorProperty: {
        set: function (x) {
            document.write("in property set accessor" + newLine);
            this.newaccpropvalue = x;
        },
        get: function () {
            document.write("in property get accessor" + newLine);
            return this.newaccpropvalue;
        },
        enumerable: true,
        configurable: true
    }
})
console.log(obj)    //{ newDataProperty: 101, newAccessorProperty: [Getter/Setter] }
```


```javascript
var obj = Object.create({a:1},{
    x: {
        value: 1,
        writable: true,
        enumerable: true,
        configurable: true
    }
})
console.log(obj)            //{x: 1} #注：Chrome控制台会将对象的__proto__中的键值进行输出，而nodejs不会。
console.log(obj.__proto__)  //{a:1}
```

### Object.getPrototypeOf(obj)

返回对象的原型

```javascript
var A = function(){this.x = 1}
A.prototype.y = 2
var a = new A()
console.log(Object.getPrototypeOf(a))   //{y:2}
```

### Object.getOwnPropertyNames(obj)

返回对象自己的属性的名称。一个对象的自己的属性是指直接对该对象定义的属性，而不是从该对象的原型继承的属性。 对象的属性包括字段（对象）和函数。

```javascript
var A = function(){this.x = 1}
A.prototype.y = 2
var a = new A()
console.log(Object.getOwnPropertyNames(a))   //['x']
```

### Object.isSealed(obj)

如果无法在对象中修改现有属性的特性，且无法向对象添加新属性，则返回 true。

### Object.isFrozen(obj)

如果无法在对象中修改现有属性的特性和值，且无法向对象添加新属性，则返回 true。

#### Object.isExtensible(obj)

如果可以向对象添加新属性，则返回true