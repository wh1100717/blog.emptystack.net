title: "Stylus和Less中颜色计算公式的差异"
date: "2015-07-05"
excerpt: true
comments: true
tags:
    - stylus
    - less
---


在写Stylus的时候会发现其 `lighten/darken` 函数和Less中得到的结果是不同的，深入了解后发现其实这是一个很有趣的问题。实际上Less/Sass对于lighten/darken的只提供了数值型的函数实现，然而输入参数却要求百分比的形式(非常奇怪的设定)。而Stylus提供了数值型、百分比类型的lighten/darken函数标准。
具体颜色计算算法如下：

###Stylus:

```stylus
hsl(0deg, 0%, 17%)          // #2b2b2b
lighten(#2b2b2b, 10)        // #454545
hsl(0deg, 0%, 27%)          // #454545
lighten(#2b2b2b, 10%)       // #404040
hsl(0deg, 0%, 25%)          // #404040
```

Stylus将颜色转换为hsl进行计算。如果为数值类型计算公式为`lightness += amount`，如果为百分比类型则`lightness += (1 - lightness) * percent`

根据上例所示，执行`lighten(#2b2b2b, 10)`实际上是在其hsl模式下增加了lightness属性增加了10个百分点，变为27%。而执行`lighten(#2b2b2b, 10%)` 则执行了 `0.17 + (1 - 0.17) * 10% = 0.253`，四舍五入后为25%。

<!-- more -->

###Less

```stylus
hsl(0deg, 0%, 17%)          // #2b2b2b
lighten(#2b2b2b, 10%)       // #454545
hsl(0deg, 0%, 27%)          // $454545
```

从上面代码可以看出，Less在计算lightness时，计算公式为`lightness += percent * 100` 即与Stylus中amount类型相同。

###结论

Less实际上只提供了数值类型输入的函数(虽然输入形式上于Stylus中百分比类型相同，但实际效果与Stylus中数值类型相同)，而Stylus提供了两种类型的输入，因此如果要将Less项目重构为Stylus类型，则Less中`lighten(#2b2b2b, 10%)` 与stylus中`lighten(#2b2b2b, 10)`等同。

