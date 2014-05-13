---
layout: default
title: SVG初步
permalink: "the-svg-primer.html"
---

D3最擅长于生成和操纵SVG图像来进行可视化。绘制`div`或其它原生HTML元素也是可以的，但是考虑到不同浏览器之间总会存在一些标准上的差异，使用SVG会更可靠更稳定，也会更快一些。

类似于Illustrator的矢量绘图软件可以生成SVG文件，但是我们这里讨论的是如何用代码来生成。

## SVG元素
可扩展矢量图形(Scalable Vector Graphics, SVG)是一种基于文本的图像格式。每个SVG图像都由类似于HTML的标签代码来定义。SVG代码可以直接用于HTML文档中。每个网页浏览器都是支持SVG的([除了IE8及之前的版本](http://caniuse.com/#feat=svg))。SVG是基于XML的，因此你会发现没有结束标签的元素必须是自封闭的。比如

{% highlight xml %}
<element></element>   <!-- Uses closing tag -->
<element/>            <!-- Self-closing tag -->
{% endhighlight %}

在你开始画之前，你必须先生成一个初始的SVG元素。将此元素视为一个画布，你将要在此画布上大展身手(从这个意义看，SVG的这个元素类似于HTML的`canvas`元素)。即使在一个最小的示例中，你最好也指定一下`width`和`height`。如果你没有指定宽度和高度，它会占用`svg`所在元素中的尽可能多的空间。

{% highlight html %}
<svg width="500" height="50">
</svg>
{% endhighlight %}

下面给出效果图

<svg width="500" height="50">
</svg>

没看到? 尝试在空白位置点击右键，再选择"Inspect Element"，你的浏览器应该得到类似于下面的结果

![](images/100-an-svg-primer-1.png)

注意，这里确实有一个`svg`元素，而它刚好就占用了宽为500像素高为50像素的一个区域。只不过，空白的图像确实没什么好看的。

还需注意的是，浏览器默认`像素`为数值单位。我们用的是`500`和`50`，而无需用`500px`和`50px`。当然你也可以显式地指定`px`，或任意其它浏览器支持的单位，比如`em`, `pt`, `in`, `cm`和`mm`。

## 简单形状
在`svg`标签中，你可以使用大量的图形元素，包括`rect`，`circle`，`ellipse`, `line`，`text`和`path`。

如果你熟悉计算机图形编程，你会了解通常的基于像素的坐标系统的原点`0,0`位于绘制区域的左上角。增加`x`值会导致右移，增加`y`值会导致下移。

<svg width="505" height="65">
    <line x1="5" y1="5" x2="5" y2="50" stroke="gray" stroke-width="1"/>
    <line x1="5" y1="50" x2="0" y2="45" stroke="gray" stroke-width="1"/>
    <line x1="5" y1="50" x2="10" y2="45" stroke="gray" stroke-width="1"/>
    <line x1="5" y1="5" x2="500" y2="5" stroke="gray" stroke-width="1"/>
    <line x1="500" y1="5" x2="495" y2="0" stroke="gray" stroke-width="1"/>
    <line x1="500" y1="5" x2="495" y2="10" stroke="gray" stroke-width="1"/>
    <circle cx="5" cy="5" r="3" fill="#008"/>
    <text x="10" y="20">0,0</text>
    <circle cx="105" cy="25" r="3" fill="#008"/>
    <text x="110" y="40">100,20</text>
    <circle cx="205" cy="45" r="3" fill="#008"/>
    <text x="210" y="60">200,40</text>
</svg>

`rect`元素会画出一个矩形。可以用`x`和`y`来指定它的左上角的位置，而用`width`和`height`指定其大小。下面的这个矩形填充了我们的整个SVG画布。

{% highlight html %}
    <rect x="0" y="0" width="500" height="50"/>
{% endhighlight %}
<svg width="500" height="50">
    <rect x="0" y="0" width="500" height="50"/>
</svg>

`circle`元素会画出一个圆。可以用`cx`和`cy`来指定圆心位置，而用`r`来指定其半径。下面的这个圆位于我们的500个像素宽的画布的中心，所以它的`cx`值为250。

{% highlight html %}
    <circle cx="250" cy="25" r="25"/>
{% endhighlight %}

<svg width="500" height="50">
    <circle cx="250" cy="25" r="25"/>
</svg>

`ellipse`元素是类似的，只不过它在每个轴上的半径可以不一样。因此，半径`r`变成了`rx`和`ry`。

{% highlight html %}
    <ellipse cx="250" cy="25" rx="100" ry="25"/>
{% endhighlight %}

<svg width="500" height="50">
    <ellipse cx="250" cy="25" rx="100" ry="25"/>
</svg>

`line`元素用来画一条线段。它使用`x1`和`y1`指定线段的一个端点位置，而用`x2`和`y2`来指定另外一端的位置。笔画`stroke`的颜色也必须指定，不然就混在背景也看不见了。

{% highlight html %}
    <line x1="0" y1="0" x2="500" y2="50" stroke="black"/>
{% endhighlight %}

<svg width="500" height="50">
    <line x1="0" y1="0" x2="500" y2="50" stroke="black"/>
</svg>

`text`用于渲染文本。它使用`x`指定文本左边界的位置，用`y`来指定基线(baseline)的垂直坐标。

{% highlight html %}
    <text x="250" y="25">Easy-peasy</text>
{% endhighlight %}
<svg width="500" height="50">
    <text x="250" y="25">Easy-peasy</text>
</svg>

除非显式地指定(后面马上会详细介绍如何指定文本样式)，`text`会继承父元素中的CSS指定的字体样式。注意，上面例子中的文本与周围的段落的格式是保持一致的。我们可以用下面的代码修改其格式。

{% highlight html %}
    <text x="250" y="25" font-family="sans-serif"
     font-size="25" fill="gray">Easy-peasy</text>
{% endhighlight %}
<svg width="500" height="50">
    <text x="250" y="25" font-family="sans-serif"
     font-size="25" fill="gray">Easy-peasy</text>
</svg>

另外，需要提醒的是，任意到达SVG画布边界的图形元素都会被裁剪掉。所以，使用`text`时要小心，确保里面的字符不要被裁掉。如果将基线坐标(`y`)修改为50，也就是SVG的高度，你就能体会到这一点了。

{% highlight html %}
    <text x="250" y="50" font-family="sans-serif"
     font-size="25" fill="gray">Easy-peasy</text>
{% endhighlight %}
<svg width="500" height="50">
    <text x="250" y="50" font-family="sans-serif"
     font-size="25" fill="gray">Easy-peasy</text>
</svg>

`path`用于绘制更复杂的形状(比如地图中的国境线)，我会后面会单独介绍。现在，我们只讨论简单形状。

## SVG元素的样式
SVG的默认样式是黑色填充而不画线。如果你想修改样式，你需要将样式应用到元素上。常用的SVG属性有：

  - `fill` ---- 一个颜色值。和CSS一样，颜色可以有几种指定方式
    - 颜色名称。比如`orange`
	- 十六进制数。比如`#3388aa`或`#38a`
	- RGB值。比如`rgb(10,150,20)`
	- RGB值加上不透明度。`rgba(10,150,20,0.5)`
  -	`stroke` ---- 也是一个颜色值，即画线时的颜色
  - `stroke-width` ---- 一个数值(一般是以像素为单位)
  - `opacity` ---- 0到1之间的一个数值，0表示完全透明，1表示完全不透明

通过`text`，你可以使用下面这些属性，它们含义和CSS是保持一致的。

  - `font-family`
  - `font-size`

和CSS一样，为SVG元素指定样式也有两种方式：直接作为元素属性，或使用CSS样式规则。下面是直接将样式属性应用于`circle`的代码：

{% highlight javascript %}
<circle cx="25" cy="25" r="22"
 fill="yellow" stroke="orange" stroke-width="5"/>
{% endhighlight %}

<svg width="500" height="50">
<circle cx="25" cy="25" r="22"
 fill="yellow" stroke="orange" stroke-width="5"/>
</svg>

当然，我们可以不直接用这些样式属性，而是为`circle`指定一个样式类别(就像为HTML元素指定类别一样)。

{% highlight javascript %}
<circle cx="25" cy="25" r="22" class="pumpkin"/>
{% endhighlight %}

然后，把`fill`，`stroke`和`stroke-width`这些规则放到一个CSS样式中，这样就构成了一个新类别。

{% highlight css %}
.pumpkin {
    fill: yellow;
    stroke: orange;
    stroke-width: 5;
 }
{% endhighlight %}

使用CSS方法有一些明显的好处：

  - 样式只需定义一次，然后应用到多个元素上
  - 一般，CSS代码比元素属性代码更易懂
  - CSS方法更易控制，从而让设计更高效

不过，使用CSS来应用SVG样式还是会让一些人心里不舒服。因为`fill`，`stroke`和`stroke-width`其实都不是CSS属性(最接近的CSS属性是`background-color`和`border`)。如果你想将SVG专用的规则标记出来，你可以在选择器中加上`svg`关键字。

{% highlight css %}
svg .pumpkin {
    /* ... */
}
{% endhighlight %}

## 图层和绘制顺序
SVG中没有"层"和深度的概念。SVG也不支持CSS的`z-index`属性。所以，形状都只能是x/y平面上的二维图形。

但是，多个形状之间又常常会出现重叠。

{% highlight javascript %}
<rect x="0" y="0" width="30" height="30" fill="purple"/>
<rect x="20" y="5" width="30" height="30" fill="blue"/>
<rect x="40" y="10" width="30" height="30" fill="green"/>
<rect x="60" y="15" width="30" height="30" fill="yellow"/>
<rect x="80" y="20" width="30" height="30" fill="red"/>
{% endhighlight %}


<svg width="500" height="50">
<rect x="0" y="0" width="30" height="30" fill="purple"/>
<rect x="20" y="5" width="30" height="30" fill="blue"/>
<rect x="40" y="10" width="30" height="30" fill="green"/>
<rect x="60" y="15" width="30" height="30" fill="yellow"/>
<rect x="80" y="20" width="30" height="30" fill="red"/>
</svg>

元素的编码顺序决定了它们的深度顺序。紫色的矩形最先出现在代码中，因此它被首先画出来。然后蓝色矩形覆盖了紫色矩形，之后是绿色，依此类推。

将SVG视为画布就很好理解了。先画的总是被后画的给掩盖，因而后画的形状表现为最上面。

因此，如果有些形状不能被遮挡，则绘制顺序就很重要了。比如，坐标轴和散点图上的标签。坐标轴和标签总是应该出现在SVG代码的最后面，这样才能保证它们始终出现在其它元素的上面。

## 透明度
如果在可视化中出现重叠，而你又想让被遮挡的元素可见，或者你想强调一些元素而弱化其它一些，那么透明度就有用武之地了。

使用透明度有两种方法：使用带不透明度的RGB，或单独设置不透明度`opacity`。

你可以在任意需要颜色的地方使用`rgba()`，比如`fill`，或`stroke.rgba()`都接受3个0至255之间的颜色值(分别表示红绿蓝)和1个0.0至1.0的不透明度值。

{% highlight html %}
<circle cx="25" cy="25" r="20" fill="rgba(128, 0, 128, 1.0)"/>
<circle cx="50" cy="25" r="20" fill="rgba(0, 0, 255, 0.75)"/>
<circle cx="75" cy="25" r="20" fill="rgba(0, 255, 0, 0.5)"/>
<circle cx="100" cy="25" r="20" fill="rgba(255, 255, 0, 0.25)"/>
<circle cx="125" cy="25" r="20" fill="rgba(255, 0, 0, 0.1)"/>
{% endhighlight %}

<svg width="500" height="50">
<circle cx="25" cy="25" r="20" fill="rgba(128, 0, 128, 1.0)"/>
<circle cx="50" cy="25" r="20" fill="rgba(0, 0, 255, 0.75)"/>
<circle cx="75" cy="25" r="20" fill="rgba(0, 255, 0, 0.5)"/>
<circle cx="100" cy="25" r="20" fill="rgba(255, 255, 0, 0.25)"/>
<circle cx="125" cy="25" r="20" fill="rgba(255, 0, 0, 0.1)"/>
</svg>

注意，在用`rgba()`时，`fill`和`stroke`的不透明度是独立的。下面这个例子中，圆在填充时`fill`的不透明度是75%，而`stroke`的不透明度是25%。

{% highlight html %}
<circle cx="25" cy="25" r="20" fill="rgba(128, 0, 128, 1.0)"/>
<circle cx="50" cy="25" r="20" fill="rgba(0, 0, 255, 0.75)"/>
<circle cx="75" cy="25" r="20" fill="rgba(0, 255, 0, 0.5)"/>
<circle cx="100" cy="25" r="20" fill="rgba(255, 255, 0, 0.25)"/>
<circle cx="125" cy="25" r="20" fill="rgba(255, 0, 0, 0.1)"/>
{% endhighlight %}

<svg width="500" height="50">
<circle cx="25" cy="25" r="20" fill="rgba(128, 0, 128, 1.0)"/>
<circle cx="50" cy="25" r="20" fill="rgba(0, 0, 255, 0.75)"/>
<circle cx="75" cy="25" r="20" fill="rgba(0, 255, 0, 0.5)"/>
<circle cx="100" cy="25" r="20" fill="rgba(255, 255, 0, 0.25)"/>
<circle cx="125" cy="25" r="20" fill="rgba(255, 0, 0, 0.1)"/>
</svg>

如果要为整个元素设置不透明度，可以使用`opacity`属性。下面的例子是完全不透明的圆。

<svg width="500" height="50">
    <circle cx="25" cy="25" r="20" fill="purple" stroke="green" stroke-width="10"/>
    <circle cx="65" cy="25" r="20" fill="green" stroke="blue" stroke-width="10"/>
    <circle cx="105" cy="25" r="20" fill="yellow" stroke="red" stroke-width="10"/>
</svg>

同样是这些圆，设置不同的`opacity`之后，结果就会变化。

{% highlight html %}
<circle cx="25" cy="25" r="20" fill="purple" 
        stroke="green" stroke-width="10"
        opacity="0.9"/>
<circle cx="65" cy="25" r="20" fill="green"
        stroke="blue" stroke-width="10"
        opacity="0.5"/>
<circle cx="105" cy="25" r="20" fill="yellow"
        stroke="red" stroke-width="10"
        opacity="0.1"/>
{% endhighlight %}

<svg width="500" height="50">
<circle cx="25" cy="25" r="20" fill="purple" stroke="green" stroke-width="10" opacity="0.9"/>
<circle cx="65" cy="25" r="20" fill="green" stroke="blue" stroke-width="10" opacity="0.5"/>
<circle cx="105" cy="25" r="20" fill="yellow" stroke="red" stroke-width="10" opacity="0.1"/>
</svg>

你也可以在使用`rgba()`的同时再设置元素的`opacity`。这时，不透明度会相乘。下面例子中的圆对于`fill`和`stroke`使用同样的RGBA值，第一个圆没有设置`opacity`，而后两个设置了。

<circle cx="25" cy="25" r="20"
        fill="rgba(128, 0, 128, 0.75)" 
        stroke="rgba(0, 255, 0, 0.25)" stroke-width="10"/>
<circle cx="65" cy="25" r="20"
        fill="rgba(128, 0, 128, 0.75)" 
        stroke="rgba(0, 255, 0, 0.25)" stroke-width="10"
        opacity="0.5"/>
<circle cx="105" cy="25" r="20"
        fill="rgba(128, 0, 128, 0.75)" 
        stroke="rgba(0, 255, 0, 0.25)" stroke-width="10"
        opacity="0.2"/>

<svg width="500" height="50">
<circle cx="25" cy="25" r="20"
        fill="rgba(128, 0, 128, 0.75)" 
        stroke="rgba(0, 255, 0, 0.25)" stroke-width="10"/>
<circle cx="65" cy="25" r="20"
        fill="rgba(128, 0, 128, 0.75)" 
        stroke="rgba(0, 255, 0, 0.25)" stroke-width="10"
        opacity="0.5"/>
<circle cx="105" cy="25" r="20"
        fill="rgba(128, 0, 128, 0.75)" 
        stroke="rgba(0, 255, 0, 0.25)" stroke-width="10"
        opacity="0.2"/>
</svg>

注意，第3个圆的`opacity`为0.2，即20%。而它的紫色填充已经有了不透明值0.75(或75%)。因此，紫色区域最终的不透明度为0.2乘以0.75，结果为0.15，或15%。

