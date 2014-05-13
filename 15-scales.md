---
layout: default
title: 缩放
permalink: "scales.html"
---

"缩放是将输入域映射为输出范围的函数"，这是[Mike Bostock对D3缩放的定义](https://github.com/mbostock/d3/wiki/Quantitative-Scales)。

数据集中的值很有可能不会精确对应于可视化中的像元。因此，缩放提供了一种方便的方式，将数据值映射为基于可视化目的的新值。

D3缩放是可以让用户定义参数的函数。一旦生成一个缩放，你可以调用此缩放函数，传入一个数据值，然后它会返回一个缩放后的值。你可以任意定义和使用缩放。

将缩放想像成最终图像中的图形会让人感觉更容易理解，比如表示数值渐进变化的刻度线。但不要被误导了。这些刻度线只不过是坐标轴的一部分，它们本质上只不过是缩放的图形表达。一个缩放是一种数学关系，与图形输出并没有直接关系。我建议读者，将缩放与坐标轴视为两种不同但相关的元素。

这里，我们只讨论线性缩放，因为这也是最常用和最易于理解的类型。一旦你理解了线性缩放，其它缩放只是小菜一碟罢了。

## 苹果和像素
将下面的数据集想像成路边水果摊每个月售出的苹果的个数。

{% highlight javascript %}
var dataset = [ 100, 200, 300, 400, 500 ];
{% endhighlight %}

首先，这是个重大利好消息，因为水果摊每个月都会多卖出100个苹果。说明，经济在增长。为了表现这种上升势头，你希望用一个柱状图来表现苹果销量的变化趋势，每个柱子的高度表现了相应的数据值。

目前为止，我们已经将数据值直接用于显示，而没有考虑单位的差异。所以，如果卖出了500个苹果，柱子的高度应该是500个像元。

这大概是行得通的，但是下个月怎么办？届时可能会卖出600个苹果。而一年以后，卖出1800个苹果怎么办？你的可视化用户不得不购买越来越大的显示屏，只是为了看一下这些过于高的苹果柱子？

这时就显示出缩放的威力了。因为苹果不是像元(桔子也不是像元)，我们需要在它们之间进行缩放变换。

## 定义域和值域
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
## 正规化
## 创建一个比例
## 缩放散点图
## 改善散点图
## 其它方法
## 其它比例

