---
layout: default
title: 链式语法
permalink: "chaining-methods.html"
---

更新时间: 2012-12-30

------


D3巧妙地利用了一种`链式语法(chain syntax)`，眼尖的读者可以看到jQuery的痕迹。通过用句点将方法"链接"起来，你可以在一行代码中执行多个操作。这种方式高效而简单，但前提是你需要理解它的工作原理，以避免在接下来的学习中多走很长的弯路。

顺便一提，`函数(functions)`和`方法(methods)`只不过是同一概念的不同叫法，都是指接受一些参数作为输入，执行某个操作，然后返回数据作为输出的一段代码。

让我们回顾一下之前的第一行D3代码(测试页在[这里](htmls/50-chaining-methods-index.html))。

{% highlight javascript %}
d3.select("body").append("p").text("New paragraph!");
{% endhighlight %}

初看起来这句代码就像一坨屎，尤其是当你对编程一窍不通时。你需要了解的第一件事是，在JavaScript中(类似于HTML)，换行空格之类的空白字符对代码是没有影响的，所以，你可以把这句代码改写成多行，一个方法占一行，看起来更自然一点。

{% highlight javascript %}
d3.select("body")
  .append("p")
  .text("New paragraph!");
{% endhighlight %}

每个人都有自己的代码风格，只要看着舒服，尽情地使用缩进，换行和空白字符(制表符或空格)吧。

## 一次一个链接(Link)
下面我们将这一句代码大卸八块。

`d3` --- 表示引用D3对象，通过它我们才能使用D3中的方法。

`.select("body")` --- 将一个CSS选择器作为[`select()`](https://github.com/mbostock/d3/wiki/Selections#wiki-d3_select)方法的输入，它会返回DOM中第一个匹配元素的引用(`selectAll()`方法表示返回所有匹配元素)。在本例中，因为只需要得到`body`元素，而且只存在一个`body`元素，所以它会被传递给链中的下一个方法。

`.append("p")` --- [`append()`](https://github.com/mbostock/d3/wiki/Selections#wiki-append)方法生成一个指定的新的DOM元素，然后添加到当前选择的元素后(添加在`内部`)。在本例中，我们希望在`body`内部生成一个新的`p`，所以，我们将`"p"`作为输入参数，除此之外，该方法还接收了链中上一个方法传递下来的`body`元素的引用。最后，`append()`又将它新生成的元素的引用传递给下一个方法。

`.text("New paragraph!")` --- [`text()`](https://github.com/mbostock/d3/wiki/Selections#wiki-text)方法将一个字符串插入到当前所选元素的开始和结束标签之间。因为前一个方法传递了`p`元素的引用，`text()`的参数会被插入到`<p>`和`</p>`之间(注意，里面已有的内容会被覆盖)。

`;` --- 最后的分号很重要，它表示一行代码的结束。

## 传递
D3的很多方法(并非全部)都会返回一个选择(实际上是对一个选择的引用)，从而实现了方法链这种好用的技术。一般来说，一个方法会返回它刚操作的元素的引用，但并不绝对。

所以我们强调：方法链的顺序很重要。链中一个方法的输出类型必须与下一个方法的预期输入类型保持一致。如果相邻的输出和输入不匹配，那么传递过程就会终止，想象一下中学接力赛中掉棒的情形吧。

不知道每个函数具体的输入和输出类型时，[API参考文档](https://github.com/mbostock/d3/wiki/API-Reference)是你永远的朋友。它包含了每个函数的详细信息，并描述了是否返回一个选择。

## 不使用链式语法
我们的示例代码其实可以写成

{% highlight javascript %}
var body = d3.select("body");
var p = body.append("p");
p.text("New paragraph!");
{% endhighlight %}

干！更像坨屎。不过，有时候你确实愿意去拆分方法链，比如，有时候在一行中调用太多方法意义并不大。或者，你想让代码看起来更顺眼。

