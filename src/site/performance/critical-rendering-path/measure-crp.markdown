---
layout: article
title: "使用Navigation Timing衡量关键渲染路径"
description: "如果你没有办法衡量性能，你就没有办法优化性能。所幸的是，Navigation Timing API提供了必备工具来衡量关键渲染路径每一步的性能。"
introduction: "如果你没有办法衡量性能，你就没有办法优化性能。所幸的是，Navigation Timing API提供了必备工具来衡量关键渲染路径每一步的性能。"
article:
  written_on: 2014-04-01
  updated_on: 2014-04-28
  featured: true
  order: 5
collection: critical-rendering-path
key-takeaways:
  measure-crp:
    - Navigation Timing提供了细粒度的时间戳以便衡量关键渲染路径
    - 浏览器会在关键渲染路径的每一步中抛出可捕获的事件
---
{% wrap content%}

<style>
  img, video, object {
    max-width: 100%;
  }

  img.center {
    display: block;
    margin-left: auto;
    margin-right: auto;
  }
</style>

{% include modules/takeaway.liquid list=page.key-takeaways.measure-crp %}

作为有效的性能策略的基础，准确的衡量和指导必不可少。这也就是Navigation Timing API为我们提供的。

<img src="images/dom-navtiming.png" class="center" alt="Navigation Timing">

上图中的每一个标签代表了浏览器在跟踪页面加载时捕获的每一个细粒度时间戳。而且在这个例子中，我们也仅仅为大家展现了各种不同的时间戳中的一部分而已。我们暂且跳过所有和网络相关的时间戳，在后面的课程中还会详细介绍。

所以，这些时间戳到底有什么含义呢？

* **domLoading:** 这是整个加载进程开始的时间戳。浏览器从这个时间点开始解析收到的HTML页面的第一个字节。
* **domInteractive:** 标记了浏览器完成解析HTML，DOM树构建完毕的时间。
* **domContentLoaded:** 标记了DOM准备就绪且没有样式资源阻碍JavaScript执行的时间点，我们可以开始构建渲染树了。
    * 很多JavaScript框架会在这个事件发生后才开始执行它们自己的逻辑。因此浏览器会通过捕获domContentLoadedEventStart和domContentLoadedEventEnd来计算执行框架的代码逻辑需要多长时间。
* **domComplete:** 如名所示，所有的处理过程结束，所有的页面资源下载完成。浏览器窗口上表示页面还在加载的图标停止旋转。
* **loadEvent:** 作为所有页面加载的最后一步，浏览器会在此时触发onLoad时间，以便开始附加的应用逻辑。

HTML的规范中指明了每一个事件的详细信息：什么时候触发，什么条件触发之类。在我们的教程中，我们会重点着眼于和关键渲染路径相关的事件：

* **domInteractive** 表示DOM准备就绪。
* **domContentLoaded** 标志[DOM和CSSOM都准备就绪](http://calendar.perfplanet.com/2012/deciphering-the-critical-rendering-path/)。
    * 如果没有JavaScript阻塞渲染，documentContentLoaded事件会在domInteractive事件之后立即触发。
* **domComplete** 表示页面及其附属资源都已经准备就绪。

^

{% include_code _code/measure_crp.html full html %}

上面的例子咋一看可能有点晕，但是它确实已经很简单了。Navigation Timing API捕获了所有相关的时间戳，而我们的代码在等待onLoad事件 – 回忆一下上文介绍的onLoad事件，在domInteractive，domContentLoaded和domComplete之后触发 – 然后计算各个时间戳之间的间隔。
<img src="images/device-navtiming-small.png" class="center" alt="NavTiming demo">

通过上面的介绍和示例，我们现在知道了要跟踪哪些关键节点以及一些简单的展示方法。记住，相比直接将结果显示在页面上，更常见的是将这些统计信息发送到分析服务器上。
([Google Analytics能够自动完成此功能](https://support.google.com/analytics/answer/1205784?hl=en))，这是一种很有效的监控页面性能的方法，你也可以由此找出哪些页面还需要进一步优化性能。

{% include modules/nextarticle.liquid %}

{% endwrap%}
