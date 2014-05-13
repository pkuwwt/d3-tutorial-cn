---
layout: default
title: 基础
permalink: "fundamentals.html"
---

更新时间: 2012-12-30

使用D3需要理解以下概念。

## HTML
HTML(超文本标记语言, HyperText Markup Language)用于结构化网页内容。最简单的HTML页面如下

{% highlight html %}
<html>
<head>
	<title>Page Title</title>
</head>
<body>
	<h1>Page Title</h1>
	<p>This is a really interesting paragraph.</p>
</body>
</html> 
{% endhighlight %}

## DOM
文档对象模型(Document Object Model, DOM)指的是HTML的分层结构。每个尖括号中的标签称为一个元素(element)，我们用人类术语来描述元素之间的相对关系: 父亲，儿子，兄弟，祖先，后代。在上面的HTML代码中，`body`是它里面的所有元素(`h1`和`p`)的父亲，而`h1`和`p`则互为兄弟。页面中所有的元素都是`html`的后代。

网页浏览器通过解析DOM来理解页面的内容。

## CSS
层叠样式表(Cascading Style Sheets, CSS)用于为HTML页面的图形表达设计样式。一个简单的CSS样式表如下

{% highlight css %}
body {
    background-color: white;
    color: black;
}
{% endhighlight %}

