---
layout: default
title: 绘制柱状图
permalink: "drawing-divs.html"
---

现在是时候开始绘制数据了。我们仍然使用最开始的简单数据集。

{% highlight javascript %}
var dataset = [ 5, 10, 15, 20, 25 ];
{% endhighlight %}

我们下面将使用这个数据集来生成一个超级简单的柱状图。柱状图本质上是矩形，而HTML元素`<div>`是最简单的矩形绘制方式(其实，对于浏览器来说，所有元素都是矩形，所以通过现在这个例子，很容易地将`div`修改为`span`元素或其它元素)。

