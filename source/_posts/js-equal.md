title: "Javascript判断相等"
date: "2014-02-22"
excerpt: true
comments: true
tags:
    - javascript
---

原题地址：http://segmentfault.com/q/1010000000416414/a-1020000000416467

```javascript
>>> [1,2] == [1,2]
false
>>> [1,2] > [1,2]
false
>>> [1,2] >= [1,2]
true

>>> [1] == [1]
false
>>> 1 == [1]
true
```

<!-- more -->

解释一下为什么会得到以上结果？

---

非常有意思的问题~

首先，比较符号有如下几种：

`==` | `===` | `!=` | `!==` | `>` | `>=` | `<` | `<=`

先说一下`===`，其表示严格相等，首先比较类型，再比较值，如果都相同，则返回True，这个都木有什么问题的~ 比较简单，一带而过了。主要讨论下面一种比较类型：

---

接着说一下`==`，其表示probably equal. 可以理解模糊判等。
在ECMA-262中对其做了如下定义：
```
The production EqualityExpression : EqualityExpression == RelationalExpression is evaluated as follows:
1. Let lref be the result of evaluating EqualityExpression.
2. Let lval be GetValue(lref).
3. Let rref be the result of evaluating RelationalExpression.
4. Let rval be GetValue(rref).
5. Return the result of performing abstract equality comparison rval == lval. (see 11.9.3).
```
在11.9.3中具体定义了相等算法
```
The comparison x == y, where x and y are values, produces true or false. Such a comparison is performed as follows:
1.If Type(x) is the same as Type(y), then
    a. If Type(x) is Undefined, return true. 
    b. If Type(x) is Null, return true.
    c. If Type(x) is Number, then
        i. If x is NaN, return false.
        ii. If y is NaN, return false.
        iii. If x is the same Number value as y, return true.
        iv. If x is +0 and y is -0, return true.
        v. If x is -0 and y is +0, return true.
        vi. Return false.
    d. If Type(x) is String, then return true if x and y are exactly the same sequence of characters (same length and same characters in corresponding positions). Otherwise, return false.
    e. If Type(x) is Boolean, return true if x and y are both true or both false. Otherwise, return false.
    f. Return true if x and y refer to the same object. Otherwise, return false.
2. If x is null and y is undefined, return true.
3. If x is undefined and y is null, return true.
4. If Type(x) is Number and Type(y) is String,
return the result of the comparison x == ToNumber(y).
5. If Type(x) is String and Type(y) is Number,
return the result of the comparison ToNumber(x) == y.
6. If Type(x) is Boolean, return the result of the comparison ToNumber(x) == y.
7. If Type(y) is Boolean, return the result of the comparison x == ToNumber(y).
8. If Type(x) is either String or Number and Type(y) is Object,
return the result of the comparison x == ToPrimitive(y).
9. If Type(x) is Object and Type(y) is either String or Number, return the result of the comparison ToPrimitive(x) == y.
10. Return false.

NOTE 1 Given the above definition of equality:
    *   String comparison can be forced by: "" + a == "" + b.
    *   Numeric comparison can be forced by: +a == +b. 
    *   Boolean comparison can be forced by: !a == !b.

NOTE 2 The equality operators maintain the following invariants:
    *   A != B is equivalent to !(A == B).
    *   A == B is equivalent to B == A, except in the order of evaluation of A and B.

NOTE 3 The equality operator is not always transitive. For example, there might be two distinct String objects, each representing the same String value; each String object would be considered equal to the String value by the == operator, but the two String objects would not be equal to each other. For Example:
    *   new String("a") == "a" and "a" == new String("a") are both true.
    *   new String("a") == new String("a") is false.

NOTE 4 Comparison of Strings uses a simple equality test on sequences of code unit values. There is no attempt to use the more complex, semantically oriented definitions of character or string equality and collating order defined in the Unicode specification. Therefore Strings values that are canonically equal according to the Unicode standard could test as unequal. In effect this algorithm assumes that both Strings are already in normalised form.
```

请注意：`1.f`中提到`Return true if x and y refer to the same object. Otherwise, return false.`

**其表示如果x和y都指向了同一个对象返回true,否则返回false**

嗒嗒~~再来举个例子
```
new String("a") == "a"             #true
new String("a") == new String("a") #false
```
因此`[1] == [1]`实际上是两个array对象的比较，如果两个对象不指向同一个指针，那么则返回false
我们通过以下的小例子既可验证：
```
a = [1]
b = [1]
typeof a #Object
a == b   #false
a == a   #true
```

---

那么接下来解释以下，为什么`[1,2,3] >= [1,2,3]`为什么就是true了呢？

以下是比较运算符的算法定义：
```
The comparison x < y, where x and y are values, produces true, false, or undefined (which indicates that at least one operand is NaN). In addition to x and y the algorithm takes a Boolean flag named LeftFirst as a parameter. The flag is used to control the order in which operations with potentially visible side-effects are performed upon x and y. It is necessary because ECMAScript specifies left to right evaluation of expressions. The default value of LeftFirst is true and indicates that the x parameter corresponds to an expression that occurs to the left of the y parameter‘s corresponding expression. If LeftFirst is false, the reverse is the case and operations must be performed upon y before x. Such a comparison is performed as follows:

1. If the LeftFirst flag is true, then
    a. Let px be the result of calling ToPrimitive(x, hint Number).
    b. Let py be the result of calling ToPrimitive(y, hint Number). 
2. Else the order of evaluation needs to be reversed to preserve left to right evaluation
    a. Let py be the result of calling ToPrimitive(y, hint Number). 
    b. Let px be the result of calling ToPrimitive(x, hint Number).
3. If it is not the case that both Type(px) is String and Type(py) is String, then
    a. Let nx be the result of calling ToNumber(px). Because px and py are primitive values evaluation order is not important.
    b. Let ny be the result of calling ToNumber(py).
    c. If nx is NaN, return undefined.
    d. If ny is NaN, return undefined.
    e. If nx and ny are the same Number value, return false.
    f. If nx is +0 and ny is -0, return false.
    g. If nx is -0 and ny is +0, return false.
    h. If nx is +∞, return false.
    i. If ny is +∞, return true.
    g. If ny is -∞, return false.
    k. If nx is -∞, return true.
    l. If the mathematical value of nx is less than the mathematical value of ny —note that these mathematical values are both finite and not both zero—return true. Otherwise, return false.
4. Else, both px and py are Strings
    a. If py is a prefix of px, return false. (A String value p is a prefix of String value q if q can be the result of concatenating p and some other String r. Note that any String is a prefix of itself, because r
    may be the empty String.)
    b. If px is a prefix of py, return true.
    c. Let k be the smallest nonnegative integer such that the character at position k within px is different from the character at position k within py. (There must be such a k, for neither String is a prefix of the other.)
    d. Let m be the integer that is the code unit value for the character at position k within px.
    e. Let n be the integer that is the code unit value for the character at position k within py.
    f. If m < n, return true. Otherwise, return false.

NOTE 1 Step 3 differs from step 7 in the algorithm for the addition operator + (11.6.1) in using and instead of or.

NOTE 2 The comparison of Strings uses a simple lexicographic ordering on sequences of code unit values. There is no attemp to use the more complex, semantically oriented definitions of character or string equality and collating order defined in the Unicode specification. Therefore String values that are canonically equal according to the Unicode standard could test as unequal. In effect this algorithm assumes that both Strings are already in normalised form. Also, note that for strings containing supplementary characters, lexicographic ordering on sequences of UTF-16 code unit values differs from that on sequences of code point values.
```

`[1,2] > [1,2]`的比较实际上是首先转换成字符串，然后进行字符串比较。
```
[1,2] > [1,2]  #实际上转换成了'1,2' > '1,2'  返回false
[1,2] >= [1,2] #返回true
```

同样的道理，`1 == [1]`，因为`1`不是object, 所以`[1]`要转换成number来进行比较，因此返回true

---

那么如何来判断两个array是否相等呢？
```
a = [1,2,3]
b = [1,2,3]
#只要a不大于b并且a不小于b不就说明a等于b嘛，因此可以构造函数如下：
function arrays_equal(a,b) { return !(a<b || b<a); }
arrays_equal([1,2,3],[1,2,3]) #true
```

还有一个比较trick的方法
```
arraysSimilar = function(arr1, arr2) {
  return JSON.stringify(arr1.sort()) === JSON.stringify(arr2.sort());
};
```
