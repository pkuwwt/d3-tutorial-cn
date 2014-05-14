---
layout: default
title: 数据类型
permalink: "types-of-data.html"
---

更新时间: 2012-12-30

------


D3对输入数据的处理极为灵活。这次的主题是JavaScript和D3中常用的数据结构。

## 变量
变量就是数据项，它是数据的最小构造单元。变量也是所有其它数据结构的基础，即变量的不同配置构成不同的数据结构。

如果你初次学习JavaScript，首先要了解的是它是一门宽松类型(loosely typed)的语言，意思是，你不需要预先指定变量的类型。与之相对的是很多强类型(strong typed)的语言，比如Java(两者名字相近，但完全不同，基本没关系)，要求声明变量时指定类型，比如`int`,`float`,`boolean`,或`String`。

{% highlight java %}
//Declaring variables in Java
int number = 5;
float value = 12.3467;
boolean active = true;
String text = "Crystal clear";
{% endhighlight %}

而JavaScript，却是根据赋值内容自动地获取变量的类型。比如，`''`或`""`表示字符串类型，我习惯于使用双引号，但有些人则喜欢单引号。

{% highlight javascript %}
//Declaring variables in JavaScript
var number = 5;
var value = 12.3467;
var active = true;
var text = "Crystal clear";
{% endhighlight %}

老是`var`,`var`,`var`，是不是很无聊？但是却极为方便，因为我们可以在知道数据类型之前就将其声明出来。你甚至可以随时更改一个变量的类型，指定不同类型的值即可。


{% highlight javascript %}
var value = 100;
value = 99.9999;
value = false;
value = "This can't possibly work.";
value = "Argh, it does work! No errorzzzz!";
{% endhighlight %}

## 数组
数组只不过是值的序列，为方便起见，将多个对象或数值存储在一个连续的空间中，并用一个变量来表示。若不用数组，类似于下面这种为每个值声明一个变量的方法是很没效率的
{% highlight javascript %}
var numberA = 5;
var numberB = 10;
var numberC = 15;
var numberD = 20;
var numberE = 25;
{% endhighlight %}

如果用数组存储了这些值，则代码简单多了。数组内容用中括号`[]`来包围起来，数组的值之间用逗号分隔。
{% highlight javascript %}
var numbers = [ 5, 10, 15, 20, 25 ];
{% endhighlight %}

数据可视化中数组无处不在，所以你应该适应它并且非常熟练才行。访问数组中的值也使用中括号：
{% highlight javascript %}
numbers[2]  //Returns 15
{% endhighlight %}

中括号中的整数表示数组中的位置。需牢记的是，数组的位置总是从0开始的，即第1个元素是0，第2个元素是1，依此类推。
{% highlight javascript %}
numbers[0]  //Returns 5
numbers[1]  //Returns 10
numbers[2]  //Returns 15
numbers[3]  //Returns 20
numbers[4]  //Returns 25
{% endhighlight %}

有些人想出一些巧记的办法，比如，将数组视为一张具有空间位置，拥有行和列的表格
{% highlight javascript %}
 ID | Value
 ------------
  0  |  5
  1  |  10
  2  |  15
  3  |  20
  4  |  25
{% endhighlight %}

数组中可以存储任意类型的数据，而不仅仅是整数。

{% highlight javascript %}
var percentages = [ 0.55, 0.32, 0.91 ];
var names = [ "Ernie", "Bert", "Oscar" ];

percentages[1]  //Returns 0.32
names[1]        //Returns "Bert"
{% endhighlight %}

## 数组和for循环
基于编程的数据可视化肯定是少不了数组和`for()`循环的。它们是数据控的动感二重奏。(你大概不会承认自己是数据控，但是我不得不提醒一下，你现在看的这篇文档题为"数据类型"哟)

数组用一个变量组织大量的数据值。而`for()`则可以快速地循环访问数组中的每个值，然后执行相应的操作，比如，将值表达为图形形式。D3的神奇的`data()`方法本质上做了同样的事情，但有时候手动编写循环代码也是很重要的。

我不会解释`for()`循环的详细机制，因为再来一章才能说得清楚。这里，仅用一个示例来描述如何循环访问`numbers`数组。


{% highlight javascript %}
for (var i = 0; i < numbers.length; i++) {
	console.log(numbers[i]);  //Print value to console
}
{% endhighlight %}

看到`numbers.length`没有？这个语法是不是很优美？如果`numbers`长度为10，则此循环体中的代码会执行10次。如果长度为10亿，你懂的。这正是计算机擅长的：接受许多许多的指令，然后一直执行下去。这也是数据可视化的核心价值所在，你只要设计了和编码实现了可视化系统，系统会对不同的数据作出不同的响应。虽然数据可以发生变化，系统的映射规则却是不变的。


## 对象
数组可以很好地存储数据列表，但是对于更复杂的数据集，你就需要用到对象(object)了。你可以将JavaScript对象理解为定制的数据结构。我们使用大括号`{}`来表示对象。大括号中间是索引/值的二元组序列。索引和值之间用冒号`:`分隔，而二元组之间用逗号`,`分隔。
{% highlight javascript %}
var fruit = {
	kind: "grape",
	color: "red",
	quantity: 12,
	tasty: true
};
{% endhighlight %}

为了引用对象中的值，我们使用点记号，后面接上索引名称

{% highlight javascript %}
fruit.kind      //Returns "grape"
fruit.color     //Returns "red"
fruit.quantity  //Returns 12
fruit.tasty     //Returns true
{% endhighlight %}

可以理解成"值"属于"对象"。看，这儿有一个水果。你会问"这是什么类型的水果?"。而回答是`fruit.kind`，值为`"grape"`。你又问"好吃否?"。那是必须的，因为`fruit.tasty`值为`true`。

## 对象+数组
将数组和对象结合起来得到对象的数组，数组的对象，或对象的对象。根据你的数据集内容，你可以定制任意合理的数据结构。

比如，我们有了更多的水果，水果的分类体系需要相应地进行扩展。我们在最外层使用中括号`[]`，表示这是一个数组，而在里面使用大括号`{}`，里面包含的是一个对象，对象之间用逗号隔开。

{% highlight javascript %}
var fruits = [
    {
        kind: "grape",
        color: "red",
        quantity: 12,
        tasty: true
    },
    {
        kind: "kiwi",
        color: "brown",
        quantity: 98,
        tasty: true
    },
    {
        kind: "banana",
        color: "yellow",
        quantity: 0,
        tasty: true
    }
];
{% endhighlight %}

访问这个数据时，我们只需要根据索引就可以获得其中一个对象的值。强调一下，`[]`表示数组，`{}`表示对象。因此，`fruits`是一个数组，因此，首先要用索引获得数组中的一个元素：

{% highlight javascript %}
fruit[1]
{% endhighlight %}

接下来，因为每个数组元素都是一个对象，因此可进一步用句点来获得对象的成员：

{% highlight javascript %}
fruit[1].quantity   // Returns 98
{% endhighlight %}

下面的映射给出了`fruits`对象数组中每一个值

{% highlight javascript %}
fruits[0].kind      ==  "grape"
fruits[0].color     ==  "red"
fruits[0].quantity  ==  12
fruits[0].tasty     ==  true

fruits[1].kind      ==  "kiwi"
fruits[1].color     ==  "brown"
fruits[1].quantity  ==  98
fruits[1].tasty     ==  true

fruits[2].kind      ==  "banana"
fruits[2].color     ==  "yellow"
fruits[2].quantity  ==  0
fruits[2].tasty     ==  true
{% endhighlight %}

显然，我们共有`fruits[2].quantity`个香蕉。

## JSON
在你的D3生涯中，你很有可能会遇到JavaScript对象记号(JavaScript Object Notation)，详细内容参考[维基百科](http://en.wikipedia.org/wiki/Json)。不要想太多，JSON只不过是将数据组织成JavaScript对象的一种特定的语法。这种语法针对JavaScript(显然)和AJAX请求进行了优化，因而，你在基于web的API中看到很多JSON数组也就不足为奇了。相比于XML，JSON解析起来更快，也更容易，因此，D3没有理由不支持。

说了那么多，JSON的样子也不会比我们刚才见过的东西更古怪
{% highlight javascript %}
var jsonFruit = {
    "kind": "grape",
    "color": "red",
    "quantity": 12,
    "tasty": true
};
{% endhighlight %}
唯一区别在于，所有的成员索引都用双引号包围起来了，让它们本身也变成了字符串。

## GeoJSON
JSON是JavaScript对象的规范化，而GeoJSON是JSON对象的进一步规范化语法，它主要针对地理数据进行了优化。所有的GeoJSON对象都是JSON对象，而所有的JSON对象都是JavaScript对象。

[GeoJSON](http://geojson.org/geojson-spec.html)可以在地理空间中存储点(即经纬坐标)，它还可以存储形状(如线和多边形)和空间特征。如果你有大量的地理数据，你最好是将它们转换为GeoJSON，D3对GeoJSON支持地很好。

在介绍地图时，我们会详细介绍GeoJSON。现在，只要知道简单的GeoJSON数据长什么就行了。
{% highlight javascript %}
var geodata = {
    "type": "FeatureCollection",
    "features": [
        {
            "type": "Feature",
            "geometry": {
                "type": "Point",
                "coordinates": [ 150.1282427, -24.471803 ]
            },
            "properties": {
                "type": "town"
            }
        }
    ]
};
{% endhighlight %}

容易混淆的是，经度总是放在纬度前面。因此，习惯用"经/纬"思考问题，而不是"纬/经"。中文好像没这个问题，我们总是习惯说"经纬度"而不是纬经度。另外，将"经度"方法视为`x`方向也是顺理成章的吧。

