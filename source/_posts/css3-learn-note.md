title: "CSS 学习笔记"
date: "2014-03-09"
excerpt: true
comments: true
tags:
    - CSS
    - 笔记
---

### 1. css3新增选择器

后代级别选择器：

   * element element:选择<div>元素内部的所有<p>元素，div p
   * element>element:选择父元素为<div>元素的所有<p>元素，div>p
   * :only-child:匹配属于父元素中唯一的<p>元素，p:only-child
   * :nth-child(n):匹配父元素中的第2个<p>子元素（n可以是数字、关键词，odd和even是可用于匹配下标是奇数或偶数的子元素的关键词），p:nth-child(2)
   * :nth-last-child(n):匹配父元素中的倒数第2个<p>子元素，p:nth-last-child(2)
   * :first-child:选择属于父元素的第一个子元素的每个<p>元素，p:first-child
   * :last-child:选择属于父元素的最后一个子元素的每个<p>元素，p:last-child
   * :root:选择文档的根元素，:root
   * :empty:选择没有子元素的每个<p>元素（包括文本节点），p:empty

辈级别选择器：

   * element+element:选择紧接在<div>元素之后的所有<p>元素，div+p
   * element1~element2:选择前面有<p>元素的每个<ul>元素，<p>~<ul>
   * :first-of-type:匹配同级兄弟元素中的第一个<p>元素，p:first-of-type
   * :last-of-type:匹配同级兄弟元素中的最后一个<p>元素，p:last-of-type
   * :only-of-type:匹配属于同类型中只有唯一兄弟元素的<p>，p:only-of-type
   * :nth-of-type(n):匹配同类型中的第n个同级兄弟元素<p>（n可以是数字、关键词，odd和even是可用于匹配下标是奇数或偶数的子元素的关键词）,p:nth-of-type
   * :nth-last-of-type(n):匹配同类型中的倒数第n个同级兄弟元素<p>，nth-last-of-type(n)

<!-- more -->

伪类选择器：

   * :link:选择所有未被访问的链接，a:link
   * :visited:选择所有已被访问的链接，a:visited
   * :active:选择活动链接，a:active
   * :hover:选择鼠标指针位于其上的连接，a:hover
   * :focus:选择获得焦点的input元素，input:focus
   * :first-letter:选择每个<p>元素的首字母，p:first-letter
   * :first-line:选择每个<p>元素的首行，p:first-line
   * :before:在每个<p>元素的内容之前插入内容，p:before
   * :after:在每个<p>元素的内容之后插入内容，p:after
   * :target:选择当前活动的#news元素，#news:target
   * :root:选择文档的根元素，:root

属性选择器：

   * [attribute]:选择带有target属性的所有元素，[target]
   * [attribute=value]:选择target="_blank"的所有元素，[target=_blank]
   * [attribute~=value]:选择title属性包含单词flower的所有元素，[title~=flower]
   * [attribute|=value]:选择lang属性值以en开头的所有元素，[lang|=en]
   * [attribute^=value]:选择其src属性以"http"开头的每个<a>元素，a[src^="http"]
   * [attribute$=value]:选择其src属性以".pdf"结尾的每个<a>元素，a[src$=".pdf"]
   * [attribute*=value]:选择其src属性中包含"abc"子串的每个<a>元素，a[src*="abc"]

UI伪类选择器：

   * :enabled:选择每个启用的<input>元素，input:enabled
   * :disabled:选择每个禁用的<input>元素，input:disabled
   * :checked:选择每个被选中的<input>元素，input:checked
   * :not(selector):选择非<p>元素的每个元素，:not(p)
   * ::selection:选择被用户选取的元素部分，::selection

### 2. 响应式布局

响应式布局是现在很流行的一个设计理念。一个网站能够兼容多个终端，而不是为每个终端做一个特定的版本。

利用CSS3-Media Query实现响应式布局：

1.在link中使用@media:

```html
<link rel="stylesheet" type="text/css" media="screen and (max-width:600px)" href="link.css"/>
```

2.在样式表中内嵌@media:

```css

@media screen and (max-width:600px){
    selector{
        attribute:value;
    }
}
```

媒体类型：

   * all:默认，适用于所有设备
   * aural:语音合成器
   * braille:盲文反馈装置
   * handheld:手持设备
   * projection:投影仪
   * print:打印预览模式，打印页
   * screen:计算机屏幕
   * tty:电传打字机以及类似的使用等宽字符网格的媒介
   * tv:电视类型设备

可能的操作符：

   * and:逻辑与，连接设备名与选择条件
   * not:排除某种设备
   * only:限定某种设备类型
   * ,:设备列表

属性值：

   * width:定义输出设备中的页面可见区域宽度（单位一般为px）
   * heigth:定义输出设备中的页面可见区域高度（单位一般为px）
   * device-width:定义输出设备的屏幕可见宽度（单位一般为px）
   * device-heigth:定义输出设备的屏幕可见高度（单位一般为px）
   * orientation:定义height是否大于或等于width，portrait代表是，landscape代表否，即设备手持方向，portrait为横向，landscape为竖向
   * aspect-ratio:定义width与height的比率，即浏览器的长宽比
   * device-aspect-ratio:定义device-width与device-heigth的比率，即设备屏幕的长宽比
   * color:定义每一组输出设备的彩色原件个数，如果不是彩色设备，则值等于0
   * color-index:定义在输出设备的彩色查询表中的条目数，如果没有使用彩色设备，则值等于0
   * monochrome:定义在一个单色框架缓冲区中每像素包含的单色原件个数，如果不是单色设备，则值等于0
   * resolution:定义设别的分辨率，如96dpi，300dpi，118dpcm
   * scan:定义电视类设备的扫描工序，progressive逐行扫描，interlace隔行扫描
   * grid:用泪查询输出设备是否使用栅格或点阵，只有1和0才是有效值，1代表是，0代表否

### 3. css3边框 

border-radius属性：边框圆角

```css
box_round{ 
-moz-border-radius:30px; 
-webkit-border-radius:30px; 
-o-border-radius:30px; 
border-radius:30px; 
} 
//按此顺序设置每个radius的四个值。如果省略bottom-left，则与top-right相同。如果省略bottom-right，则与top-left相同。如果省略top-right，则与top-left相同。
//length:定义圆角的半径
//%:以百分比定义圆角的形状
```

box-shadow属性：边框阴影

```css
.box_shadow{ 
-moz-box-shadow:3px 3px 4px #ffffff; 
-webkit-box-shadow:3px 3px 4px #ffffff; 
box-shadow:3px 3px 4px #ffffff;
} 
//含义分别为：x轴偏移值、y轴偏移值、阴影的模糊度以及阴影颜色
```

border-image属性：使用图片作为边框

```css
div{ 
-webkit-border-image:url(border.png) 30 30 round; 
-o-border-image:url(border.png) 30 30 round; 
border-image:url(border.png) 30 30 round; } 
//border-image-source:用在边框的图片的路径
//border-image-slice:图片边框向内偏移
//border-image-width:图片边框的宽度
//border-image-outset:边框图片区域超出边框的量
//border-image-repeat:图片边框是否平铺(repeat)、铺满(round)或拉伸(stretch)
```

### 4. css3背景

background-clip属性：规定背景的绘制区域

```css
background-clip:border-box|padding-box|content-box; 
//border-box:背景被裁剪到边框盒
//padding-box:背景被剪裁到内边距框
//content-box:背景被剪裁到内容框
```

background-origin属性：规定背景图片的定位区域

```css
background-origin:padding-box|border-box|content-box; 
//padding-box:背景图像相对于内边框来定位
//border-box:背景图像相对于边框盒来定位
//content-box:背景图像相对于内容框来定位
```

background-size属性：规定背景图片的尺寸

```css
background-size:length|percentage|cover|contain; 
//length:设置背景图像的高度和宽度
//percentage:以父元素的百分比来设置背景图像的高度和宽度
//cover:把背景图像扩展至足够大，以使背景图像完全覆盖背景区域//contain:把图像扩展至最大尺寸，以使其宽度和高度完全适应内容区域
```

### 5. css3文本

text-overflow属性：规定当文本溢出包含元素时发生的事情

```css
test-overflow:clip|ellipsis|string; 
//clip:修剪文本
//ellipsis:显示省略符号来代表被修改的文本
//string:使用给定的字符串来代表被修剪的文本
```

text-shadow属性：向文本添加阴影(同border-shadow)

word-break属性：规定非中日韩文本的换行规定

```css
word-break:normal|break-all|keep-all; 
//normal:使用浏览器默认的换行规则
//break-all:允许在单词内换行
//keep-all:只能在半角空格或连字符处换行
```

word-wrap属性：允许对长的不可分割的单词进行分割并换到下一行

```css
word-wrap:normal|break-word; 
//normal:只在允许的断字点换行
//break-word:在长单词或URL地址内部进行换行
```

### 6. css3颜色和透明度

在css3中对颜色进行了很多扩展，我们不仅可以使用在css2中单rgb模式，16进制模式以及关键字模式，同时还支持rgba模式，hsl模式和hsla模式以及我们可以使用css3实现渐变色

rgba模式：

```css
background-color:(192,192,192,0.5); 
//a参数代表透明度，取值在0-1之间，不可为负值
```

hsl模式和hsla模式：

```css
background-color:hsl(120,65%,75%); 
background-color:hsla(120,65%,75%,0.3); 
//hsl是代表色调，饱和度，亮度三个通道的颜色
```

透明度：

```css
opacity:0.5; 
//用来定义元素的透明度，取值范围0-1
```

webkit核心的浏览器渐变：

```css
-webkit-gradient(linear,start_point,end_point,color-stop); //第四参数：设置渐变的位置及颜色(位置的取值从0-1),可设置多组径向渐变： 
-webkit-gradient(radial,x1 y1,r1,x2 y2,r2,color-stop);
```

moz核心的浏览器渐变：

```css
-moz-linear-gradient(10 10 90deg,rgb(25,0,0) 14%,rgb(0,0,255) 100%); 
//第一个参数：设置渐变起始位置及角度
//其余参数：设置渐变的颜色和位置
```

### 7. css3用户界面

css3多列：

   * column-count:规定元素应该被分隔的列数
   * column-gap:规定列之间的间隔
   * column-rule:设置所有column-rule-*属性的简易属性
   * column-width:规定列的宽度

rasize属性：规定是否可由用户调整元素尺寸的大小

```css
rasize:none|both|horizontal|vertical; 
//none:用户无法调整元素的尺寸
//both:用户可调整元素的高度和宽度
//horizontal:用户可调整元素的宽度
//vertical:用户可调整元素的高度
```

box-sizing属性：允许用户以特定的方式定义匹配某个区域的特定元素

```css
box-sizing:content-box|border-box|inherit; 
//content-box:宽度和高度分别应用到元素的内容框，在高度和宽度之外绘制元素的内边距和边框
//border-box:为元素设定的宽度和高度决定了元素的边框盒
//inherit:规定应从父元素继承box-sizing属性的值
```

outline属性：是给元素周围绘制轮廓外边框

```css
outline:[outline-color]||[outline-style]||[outline-width]||[outline-offset]|inherit 
//outline-color:指定轮廓边框的颜色
//outline-style:指定轮廓边框的轮廓
//outline-width:指定轮廓边框的宽度
//outline-offset:指定轮廓边框偏移位置的数值
```

### 8. 2D转换

transform属性：向元素应用2D或3D转换

   * translate(x,y)|translateX(n)|translateY(n):沿X，Y轴移动元素
   * scale(x,y)|scaleX(n)|scaleY(n):改变元素的高度，宽度
   * rotate(angle):在参数中规定旋转角度
   * skew(x-angle,y-angle)|skewX(angle)|skewY(angle):沿X，Y轴倾斜元素

transform-origin属性：允许改变被转换元素的位置

### 9. 3D转换

transform属性：

   * translate3d(x,y,z)|translateX(n)|translateY(n)|translateZ(n):移动
   * scale3d(x,y,z)|scaleX(n)|scaleY(n)|scaleY(n):缩放
   * rotate3d(x,y,z,angle)|rotateX(angle)|rotateY(angle)|rotateZ(angle):旋转

transform-style属性：规定被嵌套元素如何在3D空间中显示

   * flat：子元素将不保留其3D位置
   * preserve-3d:子元素将保留其3D位置

perspective属性：规定3D元素的透视效果
perspective-origin属性：规定3D元素的底部位置
backface-visibility属性：定义元素在不面对屏幕时是否可见

   * visible:背面可见
   * hidden:背面不可见

### 10. css3过渡与倒影

过渡：

```css
transition:property duration timing-function delay; 
//property:规定应用过渡的css属性的名称 
//duration:定义过渡效果花费的时间 
//timing-function:规定过渡效果的时间曲线 
//delay:规定过渡效果何时开始
```

倒影：

```css
box-reflect:none|direction offset?mask-box-image;
```

### 11. 动画

```css
@keyframes animationname{keyframes-selector{css-style;}} 
//animationname:定义动画名称
//keyframes-selector:动画时长的百分比，0-100%，from(与0%相同)，同(与100%相同)
//css-style:一个或多个合法的css样式属性
```

animate 属性

```css
animation:name duration timing-function delay iteration-count direction; 
//name:规定@keyframes动画的名称 
//iteration-count:规定动画被播放的次数 
//direction:规定动画是否在下一周期逆向播放
```




