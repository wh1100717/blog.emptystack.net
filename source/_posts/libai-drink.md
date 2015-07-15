title: "李白喝酒问题的算法"
date: "2014-03-04"
excerpt: true
comments: true
tags:
    - python
    - 算法
---

原题地址：http://segmentfault.com/q/1010000000443863/a-1020000000444266

SegmentFault遇到的李白喝酒问题的算法问题。

> 题目：李白街上走，提壶去买酒，遇店加一倍，见花喝一斗”，途中，遇见5次店，见了10此花，壶中原有2斗酒，最后刚好喝完酒，要求最后遇见的是花，求可能的情况有多少种？


题解：

```python
def w(f,d,h): return (1 if f==h else 0) if d==0 else (0 if f*h==0 or f>h else w(f*2, d-1, h)+w(f-1, d, h-1))
```