title: "CSS 学习笔记"
date: "2014-10-02"
excerpt: true
comments: true
tags:
    - CSS
    - 笔记
---

*   [选择符](##选择符)
*   [继承](##继承)
*   [定位元素](##定位元素)
*   [字体](##字体)

##选择符

*   上下文选择符(后代组合选择符)
    *   标签1 标签2 {声明}
    *   标签1 > 标签2 {声明}
    *   标签1 + 标签2 {声明}
    *   标签1 ~ 标签2 {声明}
    *   *: 通用选择符，其为一个通配符，可以匹配任何元素
        ```css
        /*后台选择符：标签2必须是标签1的后代元素*/
        article p {font-weight: bold;}
        /*子选择符：标签2必须是标签1的子元素*/
        section > h2 {font-style: italic;}
        /*紧邻同胞选择符: 标签2必须紧跟在标签1的后面*/
        h2 + p {font-variant: small-caps;}
        /*一般同胞选择符：标签2必须跟(不一定紧跟)在其同胞标签1后面*/
        h2 ~ p {color: red;}
        /*可以用它来构建非子选择符*/
        section * a {font-size: 1.3e;}
        ```

<!-- more -->

*   ID 和 类选择符
    *   类属性
    *   ID属性
        ```css
        .specialtext {font-style: italic;}
        /*其表示class为specialtext的p元素*/
        p.specialtext {color: red;}
        /*其表示class为specialtext元素的后代span元素*/
        p.specialtext span {font-weight: bold;}
        /*其表示class="specialtext feature"的元素*/
        .specialtext.featured {font-size: 120%;}
        /*与class使用方式基本相同*/
        #specialtext {color: red;}
        ```
*   属性选择符
    *   标签名[属性名]
    *   标签名[属性名='属性值']
        ```css
        /*选择任何带有属性名的标签名元素*/
        img[title] {border: 2px solid blue;}
        /*选择任何属性名为该属性值的标签名元素*/
        img[title='red flower'] {border: 4px solid green;}
        ```
*   伪类
    ```css
    /*如果不按照下标列的顺序来使用，可能会产生意想不到的结果*/
    /*一个冒号(:)表示伪类，双冒号(::)表示css3中新增的伪元素，目前css1和css2的伪元素可以用一个冒号，不过加以使用双冒号。但是伪类不能使用双冒号*/
    a:link {color: black;} /*未访问过的链接样式*/
    a:visited {color: gray;} /*访问过的链接样式*/
    a:hover {text-decoration: none;} /*鼠标指针悬停在链接上时的样式*/
    a:active {color: red;} /*链接正在被点击时的样式(鼠标在元素上按下，还没有释放)*/
    /* :focus伪类 元素获得焦点时的样式*/
    input:focus {border: 1px bolid blue;}
    /* :target伪类 *如果用户点击一个指向页面中其他元素的链接，则那个元素就是目标(target),可以用:target伪类选中它*/
    /**
    <a href="#more_info">More Information</a>
    <h2 id="more_info"> This is the information!</h2>
    当点击a元素跳转到h2元素时，h2元素的背景更改为#eee颜色。
    */
    #more_info:target {background: #eee}
    /**结构化伪类
    :first-child    表示选择的一组元素中的第一个元素
    :last-child     表示选择的一组元素中的最后一个元素
    :nth-child(n)   表示选择的一组元素中的第n个元素
    */
    ```
*   伪元素
    *   ::first-letter伪元素
    *   ::first-line伪元素
    *   ::before 和 ::after 伪元素
        ```css
        /*表示选择的元素中的第一个字母*/
        p::first-letter {font-size: 300%} /*表示每一段落首字母放大3倍*/
        /*表示选择的元素中的第一行*/
        p::first-line {font-variant: small-caps;}
        /*可用于在特定元素前面或后面添加特殊内容*/
        /**
            <p class="age">25</p>
        */
        p.age::before {content: "Age: ";}
        p.age::after {content: " years.";}
        /**
            Age: 25 years.
        */
        ```

##继承

层叠权重要点：

*   规则一：ID选择符 > 类选择符 > 标签选择符
*   规则二：行内样式 > 嵌入样式 > 链接样式，链接样式中，相同特指度的样式，后声明的优先级高于先声明的样式
*   规则一胜过规则二。换句话说，如果选择符更明确(特指度更高)，无论它在哪里，都会胜出。
*   设定的样式 > 继承的样式，此时不用考虑特指度(即显示设定优先)。

##定位元素

*   盒元素
    *   简写样式
    ```css
    {margin-top:5px; margin-right: 10px; margin-bottom: 12px; margin-left: 8px;}
    {margin: 5px 10px 12px 8px;}
    {border: 2px dashed red;}
    {border-left-style: dashed;}
    ```
    *   border  边框样式
    ```
    /**
    border-width: 宽度，可以使用thin, medium, thick等文本值，也可以使用除百分比和负值以外的绝对值。
    border-style: 样式，有none, hidden, dotted, dashed, solid, double, groove, ridge, inset和outset等文本值。
    border-color: 颜色，可是使用任意颜色值，包括RGB, HSL, 十六进制颜色值和颜色关键字。
    border-radius: 圆角
    */
    ```
    *   padding 内边距样式
    ```css
    p {padding: 10px}
    p {padding-left: 12px;}
    ```
    *   margin  外边距样式(同padding)
    *   层叠外边距
    垂直方向上的外边距会叠加：较宽的外边距决定两元素最终距离。
    ```css
    /**
        <p>1</p>
        <p>2</p>
        实际上1和2之间的距离不是(30+50)px，而是max(30,50)px
    */
    p {margin-top: 50px; margin-bottom: 30px;}
    ```
    而水平距离上的外边距是会相加的。
    *   盒子大小
    \> 没有宽度的盒子大小元素始终会扩展到填满其父元素的宽度为止。而添加水平边框、内边距、外边距会导致实际内容宽度减少。
    \> 设定宽度的盒子内容宽度是固定设置的值，如果增加边框、内边距和外边距会增加盒子的实际大小。
    \> 在css中如果设置的是box-sizing属性，则盒子的实际宽度是固定的，增加边框、内边距和外边距，则改变的是盒子内容的宽度。
*   浮动
    *   文本围绕图片
    ```css
    /**
        <img ... />
        <p> ...the paragraph text... </p>
    */
    img {float: left; margin: 0 4px 4px 0;}
    p {margin: 0; border: 1px solid red;}
    ```
    *   文本并排于图片(分栏)
    ```css
    img {float: left; margin: 0 4px 4px 0;}
    p {float: left; margin: 0; border: 1px solid red;}
    ```
*   围住浮动元素的三种方法
    *   为父元素添加`overflow: hidden`样式
        实际上`overflow: hidden`声明的真正用途是方式包含元素被超大内容撑大。包含元素依然保持其设定的宽度，而超大的子内容则会被容器剪裁掉。除此之外，`overflow: hidden`还有另一个作用，即它能可靠地迫使父元素包含其浮动的子元素。        但是其缺点是如果有类似于下拉菜单等需要超出元素范围的情况下，不能使用。
    *   浮动父元素
    *   添加非浮动的清除元素
    ```css
    .clear::after {
        content: ".";
        display: block;
        height: 0;
        visibility: hidden;
        clear: both;
    }
    ```
*   定位
    *   static      静态定位
    *   relative    相对定位
        ```css
        /*其表示相对于原本文档流中的位置的偏移定位方式*/
        p#specialpara {
            position: relative;
            top: 25px;
            left: 30px;
        }
        ```
    *   absolute    绝对定位
        绝对定位现对于顶级元素body进行定位
    *   fixed       固定定位
        固定定位相对于浏览器窗口进行定位
    *   定义上下文
        默认情况下绝对定位的上下文是body，但是可以通过改变他的祖先元素的定位方式为`relative`来更改上下文。
*   显示属性 display
    *   inline: 行内元素
    *   block: 块级元素
    *   none: 其元素所占据的空间被收回，就好像相关的标记根本不存在。对应的visibility属性相似，不同点在于如果吧visibility属性设置为hidden，元素会隐藏，但它占据的页面空间仍然“虚位以待”。
*   背景
    *   background-color 背景色
        ```css
        /**
        如果要改变前景色，则对应的属性是color，其会更改文本和边框的颜色。
        */
        body {
            background-color: #caebff;
        }
        ```
    *   background-image | background-repeat 背景图片相关
        ```css
        p {
            background-image: url(images/blue_circle.png);
            background-repeat: repeat; /*水平和垂直方向都重复*/
            background-repeat: repeat-x; /*只进行水平方向的重复*/
            background-repaet: repeat-y; /*只进行垂直方向的重复*/
            background-repeat: no-repeat; /*不重复，图片只出现一次*/
            background-repeat: round; /*css3属性，确保图片不被剪切，通过调整图片大小来适应背景区域*/
            background-repeat: space; /*css3属性，确保图片不被剪切，通过在图片见添加空白来适应背景区域*/
        }
        ```
    *   background-position    背景位置
        ```css
        /**
        top, left, bottom, right如字面意思
        center会更改背景元素的中心位置，相当于百分比表示法的50%
        百分比：位置计算比较复杂，总的来说0%相当于top/left/bottom/right等,50%相当于center。
        */
        div {
            background-position: top left; /*尽量不要将字面符合百分比合用*/
            background-position: center center;
            background-position-x: 0%; /*等同于background-position: left*/
        }
        ```
    *   background-size         背景尺寸
        *   50%: 缩放背景，使背景x轴宽度为合模型的50%，相当于50% auto。
        *   100px 50px: 设置北京100像素宽，50像素高。
        *   cover: 拉大图片，使其完全填满背景区；保持宽高比。
        *   contain: 缩放图片，使其恰好适合背景区；保持宽高比。
    *   background-attachment   背景粘附
        属性控制滚动元素内的背景图片是否随元素滚动而滚动。
        *   scroll，默认值，即背景图片随元素移动。
        *   fixed，则不随着元素移动而移动。
    *   background-clip     控制背景绘制区域的范围。
        *   border-box      背景被裁剪到边框盒。
        *   padding-box     背景被裁剪到内边距框。
        *   content-box     背景被裁剪到内容框。
    *   background-origin   控制背景定位区域的原点。
        *   padding-box     背景图像相对于内边距框来定位。
        *   border-box      背景图像相对于边框盒来定位。
        *   content-box     背景图像相对于内容框来定位。
    *   多背景图片
        ```css
        p {
            background: url(images/1.png) 30px -10px no-repeat,
                        url(images/2.png) 145px 0px no-repeat,
                        url(images/3.png) 140px -30px no-repeat,#ffbd75;
        }
        ```
    *   背景渐变
        渐变属性需要增加VPS(Vendor Specific Prefixes)，比如:
        ```css
        .g {
            background: linear-gradient(#e86a43, #fff);
        }
        /*保证各大浏览器的兼容性，实际应该写为：*/
        .g {
            background: -moz-linear-gradient(#e86a43, #fff);
            background: -webkit-linear-gradient(#e86a43, #fff);
            background: -ms-linear-gradient(#e86a43, #fff);
            background: -o-linear-gradient(#e86a43, #fff);
            background: linear-gradient(#e86a43, #fff);
        }
        ```
        ```css
        /*默认从上到下渐变*/
        .gradient1 {background: linear-gradient(#e86a43, #fff);}
        /*从左到右*/
        .gradient2 {background: linear-gradient(left, #e86a43, #fff);}
        /*某角度渐变*/
        .gradient3 {background: linear-gradient(-45deg, #e86a43, #fff);}
        /*50%处有一个渐变点*/
        .gradient4 {background: linear-gradient(#64d1dd, #fff 50%, #64d1dd);}
        /*20%和80%处有两个渐变点*/
        .gradient5 {background: linear-gradient(#e86a43 20%, #fff 50%, #e86a43 80%);}
        /*为同一个渐变点设置两种颜色可以得到突变效果*/
        .gradient5 {background: linear-gradient(#e86a43, #fff 25%, #64d1dd 25%, #64d1dd 75%, #fff 75%, #e86a43);}
        /*默认放射性渐变样式*/
        .gradient6 {background: radial-gradient(#fff, #64d1dd, #7aa25);}
        /*圆形渐变*/
        .gradient7 {background: radial-gradient(circle, #fff, #64d1dd, #7aa25)}
        /*指定位置的圆形渐变*/
        .gradient8 {background: radial-gradient(50px 30px, circle, #fff, #64d1dd, #7aa25);}
        ```

##字体

*   font-family 字体族
    字体栈：保证在首选字体不存在的时候可以有备用字体可以使用。
    5种通用字体：serif, sans-serif, monospace, cursive, fantasy。
*   font-size   字体大小
    *   绝对字体大小: 大小取决于设置的值，与祖先字体无关，缺点在于调整页面所有元素的字体大小时，需要逐一修改样式表中的font-size。像素
    *   相对字体大小: 大小取决于设定的值以及祖先字体大小的设定。百分比、em、rem
*   font-style  字体样式
    *   normal  常规样式，其会清除祖先继承过来的所有特殊样式。
    *   italic  斜体字
    *   oblique 倾斜文字(italic和oblique都是向右倾斜的文字, 但区别在于Italic是指斜体字，而Oblique是倾斜的文字，对于没有斜体的字体应该使用Oblique属性值来实现倾斜的文字效果.)
*   font-weight 字体粗细
    *   bold    粗体
    *   normal  同上
*   font-variant    字体变化
    *   small-caps  将所有小写英文字母变成小型大写字母。
*   简写字体属性
    ```css
    p {font: bold italic small-caps .9em helvetica, arial, sans-serif;}
    ```
    要使用字体简写，必须满足如下两条规则：
    *   必须声明`font-size`和`font-family`
    *   所有值必须按如下顺序声明:
        *   font-weight、font-style、font-variant 不分先后
        *   然后是font-size
        *   然后是font-family

##文本属性
*   text-indent     文本缩进
*   letter-spacing  字符间距
*   word-spacing    单词间距
*   text-decoration 文本装饰
    *   underline   下划线
    *   overline    上划线
    *   line-through    中划线
    *   none        清除文本样式
*   text-align      文本对齐
    *   left        左对齐
    *   right       右对齐
    *   center      居中
    *   justify     两端对齐
*   line-height     行高: 任何数字值(不用指定单位)
*   text-transform  文本转换
    *   uppercase   大写
    *   lowercase   小写
    *   capitalize  首字母大写
    *   none        正常
*   vertical-align  垂直对齐
    *   




