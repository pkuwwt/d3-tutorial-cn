---
layout: default
title: 安装
permalink: "setup.html"
---

## 下载D3
为你的D3工程创建一个新目录。在此目录中，我推荐再生成一个称为`d3`的子目录。然后将最新的`d3.v3.js`[下载](http://d3js.org/d3.v3.js)到此子目录中。到目前为止，D3的版本是3.4.2。

另外，D3还提供了一个"精简"版本，[d3.v3.min.js](http://d3js.org/d3.v3.min.js)，它将标准版本中的空白字符都删掉了，文件更小，加载更快。两个版本的功能是一样的，但一般你会使用标准版本来编写项目代码(利于调试)，然后在发布项目时使用精简版本(优化加载时间)。选择权在于你，而在本教程中，我会使用标准版本。

第三个选项是下载整个D3源码仓库，这样你不仅得到JavaScript文件，还有所有的组件的源代码。你可以首先浏览[源码仓库](https://github.com/mbostock/d3)的内容，然后再将[整个仓库](https://github.com/mbostock/d3/releases)下载为一个压缩的ZIP文件。

## 使用D3
在你的项目目录中创建一个简单的HTML页面，名为`index.html`。现在你的目录结构如下

    project-folder/
	    d3/
		    d3.v3.js
		    d3.v3.min.js(可选)
	    index.html

然后，将下面的内容拷贝到`index.html`中，它在`head`标签中引用了d3，而且在`body`中还为未来的JavaScript代码预留了空间。

{% highlight html %}
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>D3 Test</title>
        <script type="text/javascript" src="d3/d3.v3.js"></script>
    </head>
    <body>
        <script type="text/javascript">
            // Your beautiful D3 code will go here
        </script>
    </body>
</html> 
{% endhighlight %}

## 查看网页效果
在有些情况下，你可以直接在浏览器中打开HTML文件来查看效果。但是，当涉及到加载外部数据源时，更可靠的办法是使用一个本地网页服务器，然后从类似于`http//localhost:8888/`这样的网址访问你的本地网页。你可以使用类似于[MAMP](http://mamp.info/en/)的服务器软件，或者参考此[本档](https://github.com/mbostock/d3/wiki)来更快地激活临时服务器。

如果你了解python，则最简单的网页服务器莫过于在`index.html`所在目录执行

{% highlight python %}
python SimpleHTTPServer 8888
{% endhighlight %}

