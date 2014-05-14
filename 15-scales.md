---
layout: default
title: 尺度
permalink: "scales.html"
---

注意，原文中的scale既可以翻译成尺度，又包含缩放的意思。虽然在英文中是一个词，但不同的中文，需要选择不同的翻译。基本原则是，名词翻译成尺度，动作则翻译成缩放。

"尺度是将输入域映射为输出范围的函数"，这是[Mike Bostock对D3尺度的定义](https://github.com/mbostock/d3/wiki/Quantitative-Scales)。

数据集中的值很有可能不会精确对应于可视化中的像元。因此，尺度提供了一种方便的方式，将数据值映射为基于可视化目的的新值。

D3尺度是可以让用户定义参数的函数。一旦生成一个尺度，你可以调用此尺度函数，传入一个数据值，然后它会返回一个缩放后的值。你可以任意定义和使用尺度。

将尺度想像成最终图像中的图形会让人感觉更容易理解，比如表示数值渐进变化的刻度线。但不要被误导了。这些刻度线只不过是坐标轴的一部分，它们本质上只不过是尺度的图形表达。一个尺度是一种数学关系，与图形输出并没有直接关系。我建议读者，将尺度与坐标轴视为两种不同但相关的元素。

这里，我们只讨论线性尺度，因为这也是最常用和最易于理解的类型。一旦你理解了线性尺度，其它尺度只是小菜一碟罢了。

## 苹果和像素
将下面的数据集想像成路边水果摊每个月售出的苹果的个数。

{% highlight javascript %}
var dataset = [ 100, 200, 300, 400, 500 ];
{% endhighlight %}

首先，这是个重大利好消息，因为水果摊每个月都会多卖出100个苹果。说明，经济在增长。为了表现这种上升势头，你希望用一个柱状图来表现苹果销量的变化趋势，每个柱子的高度表现了相应的数据值。

目前为止，我们已经将数据值直接用于显示，而没有考虑单位的差异。所以，如果卖出了500个苹果，柱子的高度应该是500个像元。

这大概是行得通的，但是下个月怎么办？届时可能会卖出600个苹果。而一年以后，卖出1800个苹果怎么办？你的可视化用户不得不购买越来越大的显示屏，只是为了看一下这些过于高的苹果柱子？

这时就显示出尺度的威力了。因为苹果不是像元(桔子也不是像元)，我们需要在它们之间进行缩放变换。

## 输入和输出范围
尺度的输入范围是输入数据值的可能范围。以上面的苹果数据为例，输入范围可以是100和500之间(即数据集的最小和最大值)，也可以是0到500。

尺度的输出范围是输出数据的可能范围，为了方便显示，通常是以像元为单位。输出范围的确定完全取决于你，即信息可视化的设计者。如果你觉得最短的苹果柱子应该是10个像元高度，最高为350像元，则你可以将输出范围设置为10到350。

比如，创建一个输入范围为100,500，而输出范围为10,350的尺度。如果你给该尺度输入值100，则它会返回10，输入500则返回350。如果输入300，则返回180(300刚好是输入范围的中点，180刚好是输出范围的中点)。

我们可以在两个并列的轴上表达输入输出范围:

<svg width="505" height="115">
<text x="220" y="15" font-style="italic">Input domain</text>
<line x1="5" y1="30" x2="500" y2="30" stroke="gray" stroke-width="1"/>
<circle cx="5" cy="30" r="3" fill="#008"/>
<text x="8" y="48">100</text>
<circle cx="255" cy="30" r="3" fill="#008"/>
<text x="258" y="48">300</text>
<circle cx="500" cy="30" r="3" fill="#008"/>
<text x="473" y="48">500</text>
<line x1="5" y1="90" x2="500" y2="90" stroke="gray" stroke-width="1"/>
<circle cx="5" cy="90" r="3" fill="#008"/>
<text x="8" y="84">10</text>
<circle cx="255" cy="90" r="3" fill="#008"/>
<text x="258" y="84">180</text>
<circle cx="500" cy="90" r="3" fill="#008"/>
<text x="473" y="84">350</text>
<text x="220" y="110" font-style="italic">Output range</text>
</svg>

## 归一化
如果你熟悉归一化(normalization)的概念，将非常有助于理解下面的内容。

归一化是根据可能的最小和最大值，将数值映射到0到1之间的数的过程。比如，一年365天，第310对应于0.85年，或一年的85%。

在线性尺度下，D3完全可以帮你处理归一化过程，你不用再担心那些数学问题。输入值首先会被归一化，然后再缩放到输出范围中。

## 创建一个尺度
D3的尺度生成器可以通过`d3.scale`获取，再指定所需的尺度类型即可。

{% highlight javascript %}
var scale = d3.scale.linear();
{% endhighlight %}

恭喜你！你得到了一个名为`scale`的函数，它竟然是可以接受参数的。(不要被`var`所误导，记住，在JavaScript中，变量也是可以存储函数的)

{% highlight javascript %}
scale(2.5); //Returns 2.5
{% endhighlight %}

因为，我们还没有指定输入范围和输出范围，此函数只是一个简单的1:1的尺度。即，无论输入是什么，返回的值都是不变的。

我们可以通过`domain`函数将此尺度的输入范围设置为100,500，参数以数组的形式传入。

{% highlight javascript %}
scale.domain([100,500]);
{% endhighlight %}

输出范围的设置是类似的，用的是`range()`函数

{% highlight javascript %}
scale.range([10,350]);
{% endhighlight %}

上面几个步骤可以分开做，也可以合成一句

{% highlight javascript %}
var scale = d3.scale.linear()
.domain([100, 500])
.range([10, 350]);
{% endhighlight %}

不管你用哪种方式，反正都会得到一个可用的尺度。

{% highlight javascript %}
scale(100);  //Returns 10
scale(300);  //Returns 180
scale(500);  //Returns 350
{% endhighlight %}

一般，你会在`attr()`方法中调用尺度函数，或者单独调用尺度函数。下面，我们将用动态尺度函数来修改我们的散点图可视化。

## 缩放散点图
{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}

{% highlight javascript %}
{% endhighlight %}
## 改善散点图
## 其它方法
## 其它比例

