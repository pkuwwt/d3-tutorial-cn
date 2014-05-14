---
layout: default
title: 绘制SVG
permalink: "drawing-svgs.html"
---

更新时间: 2012-12-30

------


现在，我们已经熟悉了SVG图像的基本结构和元素，那么，应该怎样用数据生成图形呢？

你可能已经注意到，SVG元素的所有属性都是标签内属性的形式。即，它们在标签内表示为属性/值的二元组，比如

{% highlight html %}
<element property="value"/>
{% endhighlight %}

看起来非常像HTML吧！

{% highlight html %}
<p class="eureka">
{% endhighlight %}

之前，我们已经用了D3的常用方法`append()`和`attr()`来生成新的HTML元素，并设置它们的属性。因为SVG元素也属于DOM，因此和HTML元素没什么两样，所以，我们也可以以完全一样的方式来使用`append()`和`attr()`来生成SVG图像。


## 生成SVG
首先，我们需要生成一个SVG元素，它是所有SVG图形的容器。
{% highlight javascript %}
d3.select("body").append("svg");
{% endhighlight %}

这句代码的意思是，找到`body`元素，然后，将`svg`元素刚好插到结束标签`</body>`之前。这么写自然没什么问题，但是我建议把返回值保存下来：
{% highlight javascript %}
var svg = d3.select("body").append("svg");
{% endhighlight %}

因为D3中的大部分方法都返回它所操作的元素的引用，这里的`svg`变量是`append()`的返回的一个引用。不要把`svg`仅仅视为是一个变量，而要把它看成是刚创建的新的SVG对象。这个引用可以让你少写很多代码，因为你没必要每次都去搜索SVG，比如`d3.select("svg")`，而只需要用指定的这个`svg`对象。

{% highlight javascript %}
svg.attr("width", 500)
   .attr("height", 50);
{% endhighlight %}

当然，你完全可以将上面的两句代码合为一体。
{% highlight javascript %}
var svg = d3.select("body")
  .append("svg")
  .attr("width", 500)
  .attr("height", 50);
{% endhighlight %}

从[结果测试页](htmls/110-drawing-svgs-1.html)中什么也看不到，但是你确实可以通过`web inspector`看到DOM中有一个空的SVG元素。

为了舒服一点，我建议将宽度和高度值存为变量，放在代码开始醒目的位置，比如([测试页面](htmls/110-drawing-svgs-2.html))

{% highlight javascript %}
//Width and height
var w = 500;
var h = 50;
{% endhighlight %}

我后面所有的示例都保持了这个习惯。图像大小存成变量后，可以方便后续代码中的引用，比如

{% highlight javascript %}
var svg = d3.select("body")
.append("svg")
.attr("width", w)   // <-- Here
.attr("height", h); // <-- and here!
{% endhighlight %}

用变量存储常用数值，是编程的惯用做法。如果你向上苍请愿将整个世界变量化，我会非常乐意签上我的名字的。

## 数据驱动的形状
现在是时候画一些图形形状了。还是使用原来的简单数据集。

{% highlight javascript %}
var dataset = [ 5, 10, 15, 20, 25 ];
{% endhighlight %}

然后，使用`data()`来遍历这些数据点，让每个数据点生成一个`circle`。

{% highlight javascript %}
svg.selectAll("circle")
    .data(dataset)
	.enter()
	.append("circle");
{% endhighlight %}

再次强调，`selectAll()`返回的是所有圆的空引用(这些圆此时并不存在)，`data()`将我们的数据绑定到我们将要创建的元素上，而`enter()`返回的是新元素的占位元素的引用，最后，`append()`添加一个`circle`到DOM中。

为了便于后面的代码再次引用所有的这些圆，我们可以用一个变量将它们的引用存起来。
{% highlight javascript %}
var circles = svg.selectAll("circle")
                 .data(dataset)
	             .enter()
	             .append("circle");
{% endhighlight %}

圆倒是生成了，但它需要设置其位置和大小。下面的代码才是真正出奇迹的地方，

{% highlight javascript %}
circles.attr("cx", function(d, i) {
		return (i * 50) + 25;
	})
	.attr("cy", h/2)
	.attr("r", function(d) {
			return d;
	});
{% endhighlight %}
![](images/110-drawing-svgs-1.png)

对着[测试页面](htmls/110-drawing-svgs-3.html)膜拜一下。我们再逐行理解这句代码。

{% highlight javascript %}
circles.attr("cx", function(d, i) {
		return (i * 50) + 25;
})
{% endhighlight %}

这段代码的意思是在所有圆的引用上为每个圆设置`cx`年属性。我们的数据已经绑定到了`circle`元素上，所以对于每个`circle`，值`d`对应于数据集(5,10,15,20,25)中的某个值。而另一个参数`i`，也会自动地累加，也就是变成数据点的序号。比如，第1个圆的`i`为0，第2个为1，依此类推。这里，`i`的作用是让圆的中心坐标递增，从而从左至右排列，因为`i`的值是一直增大的。

{% highlight javascript %}
(0 * 50) + 25 returns 25
(1 * 50) + 25 returns 75
(2 * 50) + 25 returns 125
(3 * 50) + 25 returns 175
(4 * 50) + 25 returns 225
{% endhighlight %}

为了确保`i`在函数体中可用，你必须让它成为函数定义的一个参数(`function(d,i)`)。而且，前面的`d`也不能省，即使你并没有用到它。否则，函数会将第1个参数视为`d`，即数据点的值。

下一行代码是

{% highlight javascript %}
.attr("cy", h/2)
{% endhighlight %}

其中，`h`是整个SVG图像的高度，所以`h/2`是半高。这样的效果是让所有的圆都排列在图像垂直方向的中间。

{% highlight javascript %}
.attr("r", function(d) {
		return d;
});
{% endhighlight %}

最后，每个圆的半径`r`被设置为`d`，即对应的数据点的值。 

## 漂亮的颜色
填充颜色和线划颜色只是另外两种属性，所以，你可以用同样的方法来设置。比如通过下面的代码

{% highlight javascript %}
.attr("fill", "yellow")
.attr("stroke", "orange")
.attr("stroke-width", function(d) {
		return d/2;
});
{% endhighlight %}

于是，我们得到下面的图像([测试页面](htmls/110-drawing-svgs-4.html))
![](images/110-drawing-svgs-2.png)

当然，你可以混合和组合各种属性，并定制各种各样的函数来设置属性。数据可视化的技巧本来就在于选择合适的映射，以便让你的数据的视觉表现派上用场，并被用户所理解。


