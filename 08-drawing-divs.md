---
layout: default
title: 用div绘制柱状图
permalink: "drawing-divs.html"
---

更新时间: 2012-12-30

------


是时候开始绘制数据了，我们仍然使用之前的简单数据集。

{% highlight javascript %}
var dataset = [ 5, 10, 15, 20, 25 ];
{% endhighlight %}

我们下面将使用这个数据集来生成一个超级简单的柱状图。柱状图本质上是矩形，而HTML元素`<div>`是最简单的矩形绘制方式(其实，对于浏览器来说，所有元素都是矩形，所以通过现在这个例子，很容易地将`div`修改为`span`元素或其它元素)。

`div`元素看起来就像一个数据柱子。

<div style="display: inline-block;
            width: 20px;
            height: 75px;
            background-color: teal;">
</div>

{% highlight javascript %}
<div style="display: inline-block;
            width: 20px;
            height: 75px;
            background-color: teal;">
</div>
{% endhighlight %}

(对于坚持网页标准的人来说，上面的HTML代码绝对是不受欢迎的。正常情况下，我们不应该使用空的`div`元素来实现纯粹的视觉效果。鉴于，这只是个编程教程，权当是例外吧。)

因为使用的是`div`元素，它的`width`和`height`都可以用CSS样式来设置。我们的图表中的每个柱形都有着相同的显示属性(除了`height`)，所以，我把这些共同的样式归为一类(class)，并取名为`bar`。

{% highlight css %}
div.bar {
    display: inline-block;
    width: 20px;
    height: 75px;   /* We'll override this later */
    background-color: teal;
}
{% endhighlight %}

然后，每个`div`都需要设置为这个`bar`样式类，以利用我们定义的CSS规则。如果动手编写HTML代码的话，应该为

{% highlight html %}
<div class="bar"></div>
{% endhighlight %}

而通过D3为每个元素增加类别(class)，则应该使用`selection.attr()`方法。理解`attr()`和它的表亲`style()`之间的区别是很重要的。

## 设置属性
`attr()`用来设置HTML属性和它在元素上的值。一个HTML属性是任意一个属性/值二元组，它们被置于一个元素的尖括号`<>`内部。比如，下面这些HTML元素包含了总共5种属性(及相应的值)，

{% highlight html %}
<p class="caption">
<select id="country">
<img src="logo.png" width="100px" alt="Logo" />
{% endhighlight %}

下面是这5个属性及相应的值，它们都应该使用`attr()`方法来设置。

{% highlight html %}
class   |   caption
id      |   country
src     |   logo.png
width   |   100px
alt     |   Logo
{% endhighlight %}

为了将我们的`div`的类别设置为`bar`，代码应该为

{% highlight javascript %}
.attr("class", "bar")
{% endhighlight %}

## 关于class
注意，一个元素的类别(class)存在为HTML属性。反过来，类别(class)用于引用CSS样式(style)规则。这可能会引起一些混乱，因为设置`class`(从而引用类别中定义的样式)和对元素直接应用`style`是有区别的。这两者都可以用D3来实现。虽然你的可以选择你最喜欢的方式，但是我还是推荐用`class`来定义被多个元素共享的属性，而只在特例上应用`style`。(事实上，我们接下来也确实会这么干。)

另外，我还要简要提一下另一个D3方法`classed()`，它被用于从元素中快速地应用或删除类别。前面的添加类别的代码可以重写为

{% highlight javascript %}
.classed("bar", true)
{% endhighlight %}


## 回到柱状图
结合我们的数据集，将前面提及的代码拼起来，目前的完整D3代码为

{% highlight javascript %}
var dataset = [ 5, 10, 15, 20, 25 ];

d3.select("body").selectAll("div")
    .data(dataset)
    .enter()
    .append("div")
    .attr("class", "bar");
{% endhighlight %}

![](images/80-drawing-divs-1.png)

这里是[测试页面](htmls/80-drawing-divs-1.html)。一定要看一下它的源码，并打开你的web inspector来查看运行详情。你会看到5个垂直柱子，每个对应于数据集中的数据点。因为中间没有空格，所以整个看起来像个大矩形。

## 设置样式
`style()`方法用于将CSS属性和值直接应用到HTML元素上。该方法等价于将样式(style)中的CSS规则直接写到HTML元素中，比如

{% highlight html %}
<div style="height: 75px;"></div>
{% endhighlight %}

在一个柱状图中，每个柱子的高度是相应数据值的函数。所以，我们需要在D3代码后面添加一点代码。

{% highlight javascript %}
.style("height", function(d) {
    return d + "px";
});
{% endhighlight %}

![](images/80-drawing-divs-2.png)

这里是[测试结果页面](htmls/80-drawing-divs-2.html)。展现在你面前的是一个小型柱状图。

当D3遍历每个数据点时，`d`的值会被设置为相应的数据点。所以我们将`height`值设置为`d`(当前数据值)加上`px`(表示像素单元)。因此，结果应该为5px,10px,15px,20px和25px。

因为看起来依然很傻，可以让柱状图更高一些。

{% highlight javascript %}
.style("height", function(d) {
    var barHeight = d * 5;  //Scale up by factor of 5
    return barHeight + "px";
});
{% endhighlight %}

我们还可以在每个柱子右边加上一点空格，让柱状图看起来更美观。

{% highlight css %}
margin-right: 2px;
{% endhighlight %}

![](images/80-drawing-divs-3.png)

还不错，可以当作SIGGRAPH的插图了。

这里是最终的[测试页面](htmls/80-drawing-divs-3.html)。再次提醒，务必亲自认真看一下源码，并使用web inspector来比较原始HTML与最终DOM的差异。


