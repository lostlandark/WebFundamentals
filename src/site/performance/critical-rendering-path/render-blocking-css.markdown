---
layout: article
title: "呈现阻塞的CSS"
description: "默认CSS被当做一种呈现阻塞资源，这意味着浏览器将持续渲染处理内容直到CSSOM被构建。保证你的CSS精简，尽可能快地实现它，并使用媒体类型和查询解决呈现问题。"
introduction: "默认CSS被当做一种呈现阻塞资源，这意味着浏览器将持续渲染处理内容直到CSSOM被构建。保证你的CSS精简，尽可能快地实现它，并使用媒体类型和查询解决呈现问题。"
article:
  written_on: 2014-04-01
  updated_on: 2014-04-28
  order: 3
collection: critical-rendering-path
key-takeaways:
  render-blocking-css:
    - 默认下CSS被当作一个渲染阻塞资源
    - 媒体类型和媒体查询允许我们将一些CSS资源标记为非阻塞
    - 所有CSS资源，无论阻塞或非阻塞都被浏览器下载
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


在上一节里我们看到关键渲染路径需要我们使用DOM和CSSOM来构建渲染树结构，这就创建了一个重要的性能暗示：**HTML和CSS都是呈现阻塞资源。** HTML是显而易见的，由于没有DOM我们就无法进行任何呈现，但是CSS需求可能就不那么明显。当我们不阻塞CSS试图进行典型页面呈现时会发生什么？

{% include modules/takeaway.liquid list=page.key-takeaways.render-blocking-css %}

<div class="clear">
  <div class="g--half">
    <b>使用CSS的纽约时报</b>
    <img class="center" src="images/nytimes-css-device.png" alt="NYTimes with CSS">

  </div>

  <div class="g--half g--last">
    <b>不使用CSS的纽约时报(FOUC)</b>
    <img src="images/nytimes-nocss-device.png" alt="NYTimes without CSS">

  </div>
</div>

{% comment %}
<table>
<tr>
<td>使用CSS的纽约时报</td>
<td>不使用CSS的纽约时报(FOUC)</td>
</tr>
<tr>
<td><img src="images/nytimes-css-device.png" alt="NYTimes with CSS" class="center"></td>
<td><img src="images/nytimes-nocss-device.png" alt="NYTimes without CSS" class="center"></td>
</tr>
</table>
{% endcomment %}

上面的例子中，显示了《NYTimes（纽约时报）》有和没有CSS样式的情况，演示了为什么呈现被阻塞直到CSS可用——没有CSS时页面显而易见是没法使用的。事实上，右边的体验通常归类于与样式无关的Flash动画（FOUC）。因此，当浏览器将会被阻塞直到它包含了DOM和CSSOM。

> **_CSS是一种渲染阻塞资源, 在客户端尽快将文件下载以优化第一次页面呈现的时间。_**

然而，如果我们有一些CSS样式只在某些特定情况下才使用呢，比如当页面被打印，或者被投射到一个更大的显示器上时？我们要是没有阻塞这些资源的渲染就好。

CSS，media types，以及media queries能让我们解决这些用例：

{% highlight html %}
<link href="style.css" rel="stylesheet">
<link href="print.css" rel="stylesheet" media="print">
<link href="other.css" rel="stylesheet" media="(min-width: 40em)">
{% endhighlight %}

媒体查询由媒体类型以及零个或者多个检测特定媒体特性表达式组成。例如，我们的第一个样式表声明没有提供任何媒体类型或查询，因此它被运用于任何情况——也就是说，它总是被呈现。另一方面，第二个样式表只适用于打印内容——或许你想重新排列布局，改变字体等等——因此这个样式不需要在页面第一次加载时被加入到渲染中。最后，最后一个样式声明提供了媒体查询，这个能被浏览器执行：如果条件符合，浏览器将会阻塞进行渲染直到样式表被下载处理。

通过使用媒体查询，我们的演示可以根据特定的用例进行显示与打印，并且在动态条件下比如改变屏幕方向，改变大小等更多情况下进行调整。**当进行你的样式声明时，密切注意媒体类型和查询，因为它们将有很大可能在关键渲染路径上影响性能！**

让我们考虑一些实际例子:

{% highlight html %}
<link href="style.css"    rel="stylesheet">
<link href="style.css"    rel="stylesheet" media="screen">
<link href="portrait.css" rel="stylesheet" media="orientation:portrait">
<link href="print.css"    rel="stylesheet" media="print">
{% endhighlight %}

* 第一个声明是渲染阻塞以及匹配任何条件。
* 第二个声明也渲染阻塞：screen是默认类型如果你没有指定任何类型，这将隐式地指定screen类型。因此，第一次和第二次声明是等价的。
* 第三个声明含有一个动态媒体查询条件能够在页面加载时评估页面。根据设备方向将页面加载, portrait.css这个样式文件能选择渲染与否。
* 最后一个声明仅当页面被打印时才被应用，因此当页面第一次在浏览器中呈现时它不会被渲染。

最后，注意渲染阻塞指的是浏览器是否在页面初始化时需要处理该资源。在这两种情况下，CSS资源仍被浏览器下载，虽然对非阻塞资源的优先级较低。

{% include modules/nextarticle.liquid %}

{% endwrap%}
