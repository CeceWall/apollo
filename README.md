# CSS

## CSS简介

CSS（ `Cascading Style Sheets` ）用于描述文档（HTML、SVG等）如何在不同媒体设备中呈现。

HTML中的CSS可以通过以下三个方式引入

1. 外部样式表，通过`<link>`标签引入外部文件中的样式
2. 内部样式表，通过`<style>`标签直接在HTML文档中编写样式
3. 内联样式，通过DOM在元素上添加`style`属性编写样式（不推荐这种方式，违背了文档和渲染分离的原则，使代码难以维护）

CSS中两个比较重要的概念是`层叠`和`继承`

层叠指的是元素的样式可以由其本身、父元素以及多个选择器规定，元素最终的样式由上述的规则共同决定，[CSS优先级](./#css-de-you-xian-ji)部分对此有详细的说明

继承指的是应用于某个元素的一些属性值将由该元素的子元素继承，而有些则不会。

{% hint style="info" %}
[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS)是学习CSS最好的网站之一，上面有丰富的CSS资料和属性参考，缺点是MDN的组织结构较为混乱，可以通过MDN内的搜索和Google（对，一定是Google）查找文档
{% endhint %}



## CSS选择器

通过外部样式表和内部样式表编写CSS时，使用选择器可以定位文档中的元素，并给元素添加指定的规则

选择器可以分为以下几类

1. 简单选择器
2. 属性选择器
3. 伪类和伪元素选择器
4. 关系选择器
5. 使用同一规则的选择器

### 简单选择器

#### 元素选择器

使用html元素名匹配文档中的指定元素

```css
div {
    font-size: 18px;
}

span {
    color: red;
}
```

#### 类选择器

类选择器由一个点**"`.`"**跟上元素的类名组成，用于在文档中class中包含该类名的元素，一个元素可以拥有多个类

```markup
<div class="content">
    <p class="paragraph first">Hello</p>
    <p class="paragraph second">World</p>
</div>
```

```css
.first {
    font-size: 18px;
}

.second {
    font-weight: 500;
}
```

#### id选择器

id选择器使用一个"`#`"跟上元素的id名组成，用于在文档中选择id为指定名称的元素。元素的id必须是唯一的，否则会产生不可预期的结果

```markup
<div>
    <p id="first" class="paragraph">Hello</p>
    <p id="second" class="second">World</p>
</div>
```

```css
#first{
    font-size: 18px;
}

#second {
    font-weight: 500;
}
```

#### 通配符选择器

通配符选择器匹配所有元素，通常会和其他选择器搭配使用，使用通配符选择器时一定要注意，避免对页面的其他部分造成意外的影响

```css
* {
    box-sizing: border-box;
}
```

### 属性选择器

属性选择器根据元素的属性和属性值来匹配元素。属性选择器由中括号构成\(`[]`\)，里面包含属性名和可选的条件

属性选择器分为

1. 存在和值选择器
2. 字串选择器

#### 存在和值选择器

* `[attr]`：该选择器选择包含 attr 属性的所有元素
* `[attr=val]`：该选择器选择 attr 属性被赋值为 val 的所有元素。
* `[attr~=val]`：该选择器选择attr属性的的属性值包含val的所有元素

```markup
<img title="sun" />
<img title="sun sunrise" />
```

```css
// 选择属性值包含title的元素
[title] { 
    border: 1px solid black;
}
// 选择属性title的值等于sun的元素
[title=sun] { 
    width: 250px;
}
// 选择属性title的值中包含sunrise单词的元素
[title~=sunrise] {
    margin-top: 24px;
}
```

#### 字串选择器

* `[attr|=val]` : 选择attr属性的值是 `val` 或值以 `val-` 开头的元素（注意`-`字符，多用于语言选择）。
* `[attr^=val]` : 选择attr属性的值以 `val` 开头
* `[attr$=val]` : 选择attr属性的值以 `val` 结尾
* `[attr*=val]` : 选择attr属性的值中包含子字符串 `val` 的元素

```markup
<div lang="zh-CN">
    你好
<div>
<div data-name="computer_core">
CPU
</div>
<div data-cotent="rocket_fuel">
Liquid Hydrogen
</div>
<div data-type="flowers">
Rose
</div>
```

```css
// 选择lang以zh或zh-开头的元素
[lang|="zh"] { 
    color: red;
}
// 选择data-name以computer开头的元素
[data-name^="computer"] {
    font-weight: bold;
}
// 选择date-content以fuel结尾的元素
[data-content$="fuel"] {
    font-weight: bold;
}
// 选择data-type中包含owe的元素
[data-type*="owe"] {
    font-size: 32px;
}
```

### 伪类和伪元素选择器

#### 伪类选择器

伪类（pseudo-class）选择器 由一个选择器、冒号`(:)`、伪类名组成，通常用于选择处于特定状态的元素（访问过的链接、选中的checkbox、拥有焦点的input、第一个兄弟元素\)

伪类十分的多，在这里不详细说明，大家可以参考[MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-classes)来进行详细的学习

在这里补充的两点需要注意的内容

1. 有一些伪类只针对某款浏览器
2. `:nth-of-type`、`:last-child` 等一系列文档结构选择器，选择的都是目标元素的兄弟元素，并且选择器必须要能选中伪类描述的元素

```markup
<div class="content">
    <p class="paragraph">第一个段落。</p>
    <p class="paragraph">第二个段落。</p>
    <p class="paragraph">第三个段落。</p>
    <p class="paragraph">第四个段落。</p>
    <div class="paragraph">第五个段落。</div>
</div>
```

```css
// 匹配第五个段落，.paragraph可以匹配父元素的最后一个元素
.paragraph:last-child { 
  font-size: 28px;
}
// 匹配第四个元素，p可以匹配父元素的倒数第二个元素
p:nth-last-child(2) {
  color: red;
}
// 什么都匹配不到，因为p并不能匹配父元素的最后一个元素
p:last-child {
  text-decoration: underline;
}
```

#### 伪元素选择器

伪元素和伪类语法相似，是由普通选择器、两个冒号`(::)`、伪元素名组成。

同样可以参考[MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-elements)来学习伪元素的使用，与伪类不同，伪元素匹配的是其子元素

### 关系选择器

#### A B

后代选择，匹配B元素，前提为B是A的后代

```markup
<div id="a">
    <div id="b">
        <div id="c">
            abc
        <div>
    <div>
</div>
```

```css
// 可以匹配中#c，因为#c是#a的后代节点
#a #c {
    font-size: 18px;
}
```

#### A &gt; B

子元素选择，匹配B元素，前提为B为A的子元素

```markup
<div id="a">
    <div id="b">
        <div id="c">
            abc
        <div>
    <div>
</div>
```

```css
// 无法匹配中#c，因为#c不是#a的子节点
#a > #c {
    font-size: 18px;
}
// 可以匹配#b，因为#b是#a的子节点
#a > #b {
    font-weight: bolder;
}
```

#### A + B

相邻兄弟节点选择，匹配B元素，前提为B是A的下一个兄弟节点

```markup
<ul>
   <li>我是大娃</li>
   <li id="second">我是二娃</li>
   <li>我是三娃</li>
   <li>我是四娃</li>
</ul>
```

```css
// 仅匹配"我是三娃"
#second + li {
    color: yellow;
}
```

#### A ~ B

兄弟节点选择，匹配B元素，前提为B是A的兄弟节点

```markup
<ul>
   <li>我是大娃</li>
   <li id="second">我是二娃，后面的都是胖子</li>
   <li>我是三娃</li>
   <li>我是四娃</li>
</ul>
```

```css
// 匹配"我是二娃"后面的li元素
#second ~ li {
    font-size: 50px
}
```

### 使用同一规则的选择器

使用`,`（逗号）隔开多个选择器，同一规则将应用于这些选择器

```css
html, body {
    padding: 0;
    margin: 0;
    height: 100%;
}
```

## CSS的优先级

CSS的优先级指的是当多个CSS选择器选中同一个元素时，权重高的规则会覆盖权重低的规则

使用如下方法计算：

* 计算规则中**id选择器**的个数A
* 计算规则中**类选择器**、**属性选择器**、**伪类选择器**的个数B
* 计算规则中**类型选择器**、**伪元素选择器**的个数C

**通配选择符**\(`*`\), **关系选择符** \(`+`, `>`, `~`, ' ``'\)  和 **否定伪类**\(`:not()`\) 对优先级没有影响

然后依次比较A、B、C来确定两条规则的优先级，即先比较A，个数A较大的规则权重较高，如果A一样大则比较B，B一样大则比较C

当A、B、C一样大的时候，后面出现的规则会覆盖前面的规则

注意有两种情况会无视选择器的优先级

1. **内联样式**会覆盖任何外部样式
2. 给规则添加**!important**时规则拥有最高的优先级

{% hint style="warning" %}
随意使用**!important**是个坏习惯，当使用**!important**时，应该优先考虑通过调整优先级来处理问题
{% endhint %}

## CSS盒模型

### 盒模型介绍

在CSS中元素被表示为一个矩形的方框，其中包含内容、内边距、边界和外边距。浏览器渲染网页时，它会算出每个元素框的内容要用什么样式，周围的元素框有多大，以及框相对于其它框放在哪里。盒模型是理解CSS布局的基础。

![&#x76D2;&#x6A21;&#x578B;&#x793A;&#x610F;&#x56FE;](.gitbook/assets/box-model-standard-small.png)

从内到位，一个CSS盒子由以下内容组成（`box-sizing`为`content-box`时）：

1. `width`, `height` 设置内容框的大小，内容框里可以显示文本或是嵌套其他盒子
2. `padding`设置内边距大小，内边距位于内容框和边框中间
3. `border`设置边框的大小，边框位于内边距与外边距中间，可以通过CSS一次或单独设置边框的`样式`、`宽度`、`颜色`
4. `margin`外边距的大小，外边距表示当前盒子与其他盒子之间的边距

{% hint style="info" %}
块级元素的上下边距可能会产生外边距塌陷（margin collapse）的现象，详情请参考[外边距塌陷](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing)文档。此外可以通过创建BFC来避免塌陷，详情参考[BFC（Block Formatting Context）](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)
{% endhint %}

### 盒模型的box-sizing

通过设置box-sizing可以设置浏览器如何计算盒子的`width`和`height`

* `content-box`，默认，`width`和`height`指定的是上图中`content`的大小。
* `border-box`，`width`和`height`指定的是上图中`content`、`padding`、`border`的大小

### 盒模型的overflow

当设置盒子的`height`和`width`不足以放置内容时，可以使用`overflow`选项来控制在这种情况下如何渲染

* `visible`，默认选项，过多的内容显示在盒子外面
* `auto`，过多的内容被隐藏，通过滚动条滚动来查看过多的内容
* `hidden`， 过多的内容被隐藏

### 盒模型的背景裁剪

通过一个名为background-clip的属性，可以设置背景显示在盒模型中的区域

* border-box，默认值，背景延伸至盒子border
* padding-box，背景延伸至padding
* content-box， 背景只在content中显示

### 盒模型的display类型

上面讲的都是`块级元素`（`display: block`）的行为，通过设置不通的display属性，会有一些表现不同的行为。这部分内容留到下文的布局部分进行详细解释

## 布局

上面讲的所有内容都是为了CSS的布局，这里有一篇较好的[布局入门手册](http://zh.learnlayout.com/toc.html)，通过这篇手册大家可以掌握基本的布局技巧，但是CSS历史悠久而且属性极多因此布局中会遇到各种各样奇奇怪怪的情况，只有通过不断得练习和积累才能较好的完成布局

