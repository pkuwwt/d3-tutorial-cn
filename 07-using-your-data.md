---
layout: default
title: 使用数据
permalink: "using-your-data.html"
---

更新时间: 2012-12-30

------


一旦你加载了数据，并将其绑定到新生成的DOM元素上之后，你要如何用它呢？下面是上次示例中的代码。

{% highlight javascript %}
var dataset = [ 5, 10, 15, 20, 25 ];

d3.select("body").selectAll("p")
    .data(dataset)
    .enter()
    .append("p")
    .text("New paragraph!");
{% endhighlight %}

如果将最后一行改成
{% highlight javascript %}
    .text(function(d) { return d; });
{% endhighlight %}

再看一下新的[测试页面](htmls/70-using-your-data-1.html)。

哇噢！我们的数据变成了每个段落的内容，这都要归功于`data()`方法。你看，将方法链接到一起之后，每次调用完`data()`方法都会生成一个接受`d`作为输入的匿名函数。对于每个当前元素，`data()`方法确保了`d`被设置为原始数据集中的对应值。

"当前元素"的值随着D3遍历每个元素而改变。比如，当遍历到第3个占位元素时，代码生成了第3个`p`标签，而`d`被设置为数据集中的第3个值(即`dataset[2]`)。所以，第3个段落的文本内容为"15"。

## 增强函数功能

考虑到有些读者可能从未编写过函数(即方法)，下面给出一个函数定义的基本结构：

{% highlight javascript %}
function(input_value) {
    //Calculate something here
    return output_value;
}
{% endhighlight %}

上面的示例中使用的函数已经没法再简单了，不值一提。

{% highlight javascript %}
function(d) {
    return d;
}
{% endhighlight %}

它被嵌入到D3的`text()`函数中，所以此匿名函数的返回值会传递给`text()`。

不过，我们可以更酷一点，因为，这些匿名函数是随意定制的。当然，编写自己的JavaScript代码有可能是乐趣，也有可能是痛苦。但不管怎样，重点在于，我们可以自由地定制了。比如，你可以增加一些其它的文字，下面的修改会产生[新的结果](htmls/70-using-your-data-2.html)。

{% highlight javascript %}
.text(function(d) {
    return "I can count up to " + d;
});
{% endhighlight %}


## 数据渴望一个怀抱

你可能会奇怪，为什么不直接用`d`的值，反而要写一个古怪的`function(d)...`呢。比如，下面的代码是不行的。

{% highlight javascript %}
.text("I can count up to " + d);
{% endhighlight %}

在我们的例子中，如果不将`d`用匿名函数管起来，它本身是没有值的。你可以将`d`想象成一个孤独的小占位符，需要一个善良关心他的函数括号的拥抱(接受匿名函数这个危险陌生人的拥抱显得比较诡异，因此不要对这个比喻想太多)。

注意，这种语法中，`d`被匿名函数的一对括号温柔地包围着，而整个匿名函数都是被`text()`方法的括号所包围的。

{% highlight javascript %}
.text(function(d) {  // <-- Note tender embrace at left
    return "I can count up to " + d;
});
{% endhighlight %}

能够使用这种语法的原因在于，`text()`, `attr()`等许多D3方法支持将一个函数作为一个参数。比如，`text()`的参数可以是一个静态的字符串

{% highlight javascript %}
.text("someString")
{% endhighlight %}

或一个函数的返回值

{% highlight javascript %}
.text(someFunction())
{% endhighlight %}

或者接受一个参数的匿名函数
{% highlight javascript %}
.text(function(d) {
	return d;
})
{% endhighlight %}

我们之前的例子中就是定义了一个匿名函数。当D3发现一个匿名函数时，它会尝试`调用`这个函数，同时把当前数据`d`作为此函数的参数传递给它。如果没有这样一个函数，D3不可能知道如何传递这样一个参数`d`。

起先，你会觉得这种语法看起来好傻，仅仅为了得到一个`d`多做了好多无用功，但这种方法的价值在于它可以变得非常复杂。不信？走着瞧。

## 不仅仅是文本
当我们考虑D3除`text()`之外的其它方法，比如`attr()`和`style()`时，事情会有趣地多。这两个函数分别用于设置所选元素的HTML属性和CSS属性。

比如，在我们上面的代码中增加一行会得到不一样的[结果](htmls/70-using-your-data-3.html)。

{% highlight javascript %}
.style("color", "red");
{% endhighlight %}

现在，所有的文字都变红了，还不错。我们甚至还可以定制一个函数来只把指定的文本染红，比如数值超过某个值的那些。所以，我们可以将函数改写成：
{% highlight javascript %}
.style("color", function(d) {
    if (d > 15) {   //Threshold of 15
        return "red";
    } else {
        return "black";
    }
});
{% endhighlight %}

[这里](htmls/70-using-your-data-4.html)是显示效果。注意，前3行仍然是黑的，一旦`d`值超过了阈值15，文本就开始变红了。

在下一个教程中，我们会使用`attr()`和`style()`来操纵`div`标签，以获得简单的柱状图。这将是我们第一个可视化结果。


