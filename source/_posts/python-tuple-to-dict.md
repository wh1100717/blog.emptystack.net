title: "python 数据结构转换,将线性元祖转换成字典树"
date: "2014-02-22"
excerpt: true
comments: true
tags:
    - python
    - 算法
---

原题地址：http://segmentfault.com/q/1010000000415526/a-1020000000415991

有一个数据表 

    id   fid        title
    1    -1          python
    2    -1          ruby
    3    -1          php
    4    -1          lisp
    5     1          flask
    6     1          django
    7     1          webpy
    8     2          rails
    9     3          zend
    10    6          dblog

<!-- more -->

这是一个栏目表,title 是栏目名称, id 是栏目 id, fid 是栏目的父id.构建一个多级栏目分类

通过 python 查询处理,会得到下面的一个元祖,

        t = (
        (1, -1, 'python'),
        (2, -1, 'ruby'),
        (3, -1, 'php'),
        (4, -1, 'lisp'),
        (5,  1, 'flask'),
        (6,  1, 'django'),
        (7,  1, 'webpy'),
        (8,  2, 'rails'),
        (9,  3, 'zend'),
        (10, 6, 'dblog')
    )

希望通过 python 处理,转换成下面的 list 字典树

        l = [
        {
            'id': 1,
            'fid': -1,
            'title': 'python',
            'son': [
                {
                    'id': 5,
                    'fid': 1,
                    'title': 'flask',
                },
                {
                    'id': 6,
                    'fid': 1,
                    'title': 'django',
                    'son': [
                        {
                            'id': 10,
                            'fid': 6,
                            'title': 'dblog',
                        },
                    ]
                },
                {
                    'id': 7,
                    'fid': 1,
                    'title': 'webpy',
                },
            ]
        },
        {
            'id': 2,
            'fid': -1,
            'title': 'ruby',
            'son': [
                {
                    'id': 8,
                    'fid': 2,
                    'title': 'rails',
                },
            ]
        },
        {
            'id': 3,
            'fid': -1,
            'title': 'php',
            'son': [
                {
                    'id': 9,
                    'fid': 3,
                    'title': 'zend',
                },
            ]
        },
        {
            'id': 4,
            'fid': -1,
            'title': 'lisp',
        }
    ]

也就是类似网站的目录, 父栏目包含子栏目.
自己写了好几个,感觉效率不够好, 求大神更 pythonic 的方法

在 stackoverflow 有人回答 大概如下：

    from pprint import pprint

    l = []
    entries = {}

    for id, fid, title in t:
         entries[id] = entry = {'id': id, 'fid': fid, 'title': title}
         if fid == -1:
              l.append(entry)
         else:
              parent = entries[fid]
              parent.setdefault('son', []).append(entry)

    pprint(l)


---

我这里先问个问题，查询的元组pid是递增的吗？
也就是说`parent = entries[fid]`一定是能取到值的呗？

---

StackOverFlow的大神答案已经很效率了吧。。虽然是错的...如果你的数据是乱序的话，你贴的代码输出的结果就不正确了。
虽然不知道你说的`pythonic`是啥...如果以看不懂为目的的话，我写了一个...
无论如何，不建议使用这种代码习惯，清晰可读是所有编程的通性，如果为了玩的话，你可以去codeforece上看看各路大神解题的答案。

---

```
e, l = {d[0]:{'id': d[0], 'fid': d[1], 'title': d[2]} for d in t}, []
for i in e: l.append(e[i]) if e[i]['fid'] == -1 else e[e[i]['fid']].setdefault('son', []).append(e[i])
print l
```
第一行先格式化数据并存储在e中
第二行对e进行遍历，如果e['fid'] == -1  则添加到l数组中。 否则找到对应的e[e[fid]]添加其子节点。

因为l.append(e[i])是地址拷贝，所以e中的数据更改直接影响l的数据(因为都是指向同一个地址的嘛~) 具体细节可以参考我在另外一个问题里面的答案 http://segmentfault.com/q/1010000000397465#a-1020000000397966

---

以下是上面那个代码的正常版本：
```
l = []
entities= {d[0]:{'id': d[0], 'fid': d[1], 'title': d[2]} for d in t}
for e_id in entities:
  entitiy = entities[e_id]
  fid = entitiy['fid']
  if fid == -1:
    l.append(entitiy)
  else:
    entities[fid].setdefault('son',[]).append(entitiy)
print l
```
