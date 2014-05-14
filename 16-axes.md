---
layout: default
title: 坐标轴
permalink: "axes.html"
---

![](images/160-axes-1.png)

## 坐标轴介绍

## 设置坐标轴
{% highlight javascript %}
var xAxis = d3.svg.axis();
{% endhighlight %}
{% highlight javascript %}
xAxis.scale(xScale);
{% endhighlight %}
{% highlight javascript %}
xAxis.orient("bottom");
{% endhighlight %}
{% highlight javascript %}
var xAxis = d3.svg.axis()
                  .scale(xScale)
                  .orient("bottom");
{% endhighlight %}
{% highlight javascript %}
svg.append("g")
    .call(xAxis);
{% endhighlight %}
{% highlight javascript %}
svg.append("g")
    .call(d3.svg.axis()
                .scale(xScale)
                .orient("bottom"));
{% endhighlight %}
![](images/160-axes-2.png)
## 清理
{% highlight javascript %}
svg.append("g")
    .attr("class", "axis")  //Assign "axis" class
    .call(xAxis);
{% endhighlight %}
{% highlight css %}
.axis path,
.axis line {
    fill: none;
    stroke: black;
    shape-rendering: crispEdges;
}

.axis text {
    font-family: sans-serif;
    font-size: 11px;
}
{% endhighlight %}
![](images/160-axes-3.png)

{% highlight javascript %}
svg.append("g")
    .attr("class", "axis")
    .attr("transform", "translate(0," + (h - padding) + ")")
    .call(xAxis);
{% endhighlight %}
![](images/160-axes-4.png)

## 检查坐标轴刻度标记
{% highlight javascript %}
var xAxis = d3.svg.axis()
                  .scale(xScale)
                  .orient("bottom")
                  .ticks(5);  //Set rough # of ticks
{% endhighlight %}
![](images/160-axes-5.png)

## 要不要Y轴?
{% highlight javascript %}
//Define Y axis
var yAxis = d3.svg.axis()
                  .scale(yScale)
                  .orient("left")
                  .ticks(5);
{% endhighlight %}
{% highlight javascript %}
//Create Y axis
svg.append("g")
    .attr("class", "axis")
    .attr("transform", "translate(" + padding + ",0)")
    .call(yAxis);
{% endhighlight %}
![](images/160-axes-6.png)

{% highlight javascript %}
var padding = 30;
{% endhighlight %}
![](images/160-axes-7.png)

## 最后结果
{% highlight javascript %}
//Dynamic, random dataset
var dataset = [];
var numDataPoints = 50;
var xRange = Math.random() * 1000;
var yRange = Math.random() * 1000;
for (var i = 0; i < numDataPoints; i++) {
    var newNumber1 = Math.round(Math.random() * xRange);
    var newNumber2 = Math.round(Math.random() * yRange);
    dataset.push([newNumber1, newNumber2]);
}
{% endhighlight %}
![](images/160-axes-8.png)
![](images/160-axes-9.png)

## 坐标轴刻度标签的格式化

{% highlight javascript %}
var formatAsPercentage = d3.format(".1%");
{% endhighlight %}

{% highlight javascript %}
xAxis.tickFormat(formatAsPercentage);
{% endhighlight %}

![](images/160-axes-10.png)


