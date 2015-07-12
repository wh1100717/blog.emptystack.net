title: "一些实用的团队工具介绍"
date: "2014-11-04"
excerpt: true
comments: true
tags:
    - 团队
    - 工具
---

#@文档撰写
---

##Markdown介绍

Markdown 是一种方便记忆、书写的纯文本标记语言，用户可以使用这些标记符号以最小的输入代价生成极富表现力的文档：譬如您正在阅读的这份文档。它使用简单的符号标记不同的标题，分割不同的段落，**粗体** 或者 *斜体* 某些文字，更棒的是，它还可以

*   书写一个 像 $E=mc^2$ 这样的 **LaTeX公式**

$$f(x_1,x_x,\ldots,x_n) = x_1^2 + x_2^2 + \cdots + x_n^2 $$

$$\sum^{j-1}_{k=0}{\widehat{\gamma}_{kj} z_k}$$

*   高亮一段 **代码**

```python
@requires_authorization
class SomeClass:
    pass

if __name__ == '__main__':
    # A comment
    print 'hello world'
```

*   高效绘制 **流程图**

```flow
st=>start: Start
op=>operation: Your Operation
cond=>condition: Yes or No?
e=>end

st->op->cond
cond(yes)->e
cond(no)->op
```

*   高效绘制 **序列图**

```seq
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
```

*   绘制表格

| 项目        | 价格   |  数量  |
| --------   | -----:  | :----:  |
| 计算机     | $1600 |   5     |
| 手机        |   $12   |   12   |
| 管线        |    $1    |  234  |

*   也可以 **删除** 一段文本
~~这段文字被删除了~~

*   同样也支持html标签

<table>
    <tr>
        <th rowspan="2">值班人员</th>
        <th>星期一</th>
        <th>星期二</th>
        <th>星期三</th>
    </tr>
    <tr>
        <td>李强</td>
        <td>张明</td>
        <td>王平</td>
    </tr>
</table>

*   也可以使用内嵌 **图标**

<i class="icon-weibo icon-2x"></i> <i class="icon-renren icon-2x"></i>

*   甚至可以增加注脚或者引用[^footnote]

> 这是一个引用

[^footnote]: 这是一个 *注脚* 的 **文本**。

---

##Markdown的编辑器

*   在线编辑器
    *   [cmd markdown](https://www.zybuluo.com/mdeditor)
    *   [马克飞象](http://maxiang.info/)
    *   [stackedit](https://stackedit.io/)
*   离线编辑器
    *   [Mou](http://www.mouapp.com/) for mac
    *   [MarkdownPad](http://www.markdownpad.com/) for windows
    *   [MdCharm](http://www.mdcharm.com/) for ubuntu && windows

#@团队协作工具
---

##Trello

[Trello](https://trello.com)是一款快速、高效工作流管理工具。同时还具有手机客户端，方便工作流同步及实时查看与提醒。

所谓Trello，就是一块大黑板（board），你可以往上加一条条的列表（list）贴各种纸条（card）。通过这三大元素的自由组合，你可以创建出属于自己的工作流。

比如你可以设置课程计划：
![此处输入图片的描述][1]

或者进行情人节策划：
![此处输入图片的描述][2]

小纸条除了可以在各个列表之间拖曳，还可以点开：
![此处输入图片的描述][3]

同时，协作功能也带来了很大的应用拓展性。
![此处输入图片的描述][4]

还有很多功能有待去体验
![此处输入图片的描述][5]

##Teambition
与Trello类似，[Teambition](https://www.teambition.com/)是国内一款轻量级的项目管理软件，可以进行项目或工作规划，分配任务，跟踪工作进度，在线讨论，上手十分容易和简单，并且免费。

界面非常清爽简洁。
![此处输入图片的描述][6]

提供了任务板功能，根据不同分类对任务进行归类，并可以设置紧急度和截止日期，同时还可以设置任务参与相关人员。
![此处输入图片的描述][7]

点开任务详情如图所示
![此处输入图片的描述][8]

同时组成员可以进行分享好的资料和文件
![此处输入图片的描述][9]

也可以查看个人目前的任务情况
![此处输入图片的描述][10]


  [1]: http://wh1100717.github.io/img/teambition/kechengjihua.jpg
  [2]: http://wh1100717.github.io/img/teambition/cehuaan.jpg
  [3]: http://wh1100717.github.io/img/teambition/zhankai.jpg
  [4]: http://wh1100717.github.io/img/teambition/xietong.jpg
  [5]: http://wh1100717.github.io/img/teambition/trelloshili.jpg
  [6]: http://wh1100717.github.io/img/teambition/tbzhuye.png
  [7]: http://wh1100717.github.io/img/teambition/tbrenwuban.jpeg
  [8]: http://wh1100717.github.io/img/teambition/tbxiangxi.jpeg
  [9]: http://wh1100717.github.io/img/teambition/tbrenwuqiang.jpeg
  [10]: http://wh1100717.github.io/img/teambition/tbwoderenwu.jpeg