---
layout: article
title: "渲染树形结构、布局以及绘制"
description: "在页面的加载过程中，CSSOM树和DOM树会被合并成一棵渲染树，用于计算每个可视元素的布局。同时，它也会作为绘制过程的输入参数，用于绘制屏幕上的每个像素点。优化其中的每一步都对优化页面的渲染性能至关重要。"
introduction: "在页面的加载过程中，CSSOM树和DOM树会被合并成一棵渲染树，用于计算每个可视元素的布局。同时，它也会作为绘制过程的输入参数，用于绘制屏幕上的每个像素点。优化其中的每一步都对优化页面的渲染性能至关重要。"
article:
  written_on: 2014-04-01
  updated_on: 2014-04-28
  order: 2
collection: critical-rendering-path
key-takeaways:
  render-tree-construction:
    - DOM树和CSSOM树合并在一起构成一棵渲染树
    - 渲染树只包含渲染所需要的节点
    - 布局的过程中会计算每个对象的精确位置和尺寸
    - 最后的绘制过程以渲染树为输入参数，在屏幕上画出每个像素点
notes:
  hidden:
    - "As a brief aside, note that 'visibility: hidden' is different from 'display: none'. The former makes the element invisible, but the element is still occupies space in the layout (i.e. it's rendered as an empty box), whereas the latter (display: none) removes the element entirely from the render tree such that the element is invisible and is not part of layout."
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

在前面的章节中，我们介绍了对象模型的构建，也就是根据加载的HTML和CSS代码构建DOM树和CSSOM树。然而它们本质上是两个互相独立的东西，各自描述了页面文档的一个不同方面：前者描述了内容，后者描述了应用于内容的样式规则。那么浏览器是如何将两者结合起来并在屏幕上画出每个像素点的呢？

{% include modules/takeaway.liquid list=page.key-takeaways.render-tree-construction %}

对浏览器而言，第一步便是将DOM树和CSSOM树合并成一棵渲染树。渲染树既包含了页面上所有的可视DOM节点，又包含了CSSOM中每个节点的样式信息。

<img src="images/render-tree-construction.png" alt="DOM and CSSOM are combined to create the render tree" class="center">

为了创建渲染树，浏览器大致需要以下步骤：

1. 从DOM树的根节点开始，遍历所有的可视节点。
  * 有些不可见元素（比如脚本标签，元数据标签之类）会被忽略，因为它们不影响渲染的结果。
  * 有些通过CSS隐藏掉的元素也会被忽略，比如上图中的span元素。由于该元素上显式地设置了属性“display:none”，所以不会出现在渲染树上。
1. 对于每个可视节点，从CSSOM中寻找对应的样式规则，并加于节点。
1. 输出可视的节点，以及每个节点计算后的样式。

{% include modules/remember.liquid list=page.notes.hidden %}

最终的渲染树既包含了所有可视的内容，又包含了相应的样式信息。快要大功告成了！**有了这棵渲染树，我们就能进入下一步，布局。**

在此之前，我们已经计算了什么节点是可视的以及它们对应的样式是什么，但是我们还没有计算它们在当前设备中准确的位置和尺寸。这正是布局阶段要做的的工作，该阶段在英语中也被称为“回流”。

为了算出每个对象的准确大小和位置，浏览器从渲染树的根节点开始遍历，计算页面中每个对象的几何样式。下面就让我们看一个简单的例子：

{% include_code _code/nested.html full %}

上面的页面中包含两个嵌套的div元素：第一个（父元素）设定了显示宽度为窗口的50%，第二个元素（子元素）设定了宽度为父元素的50%，即窗口宽度的25%。

<img src="images/layout-viewport.png" alt="Calculating layout information" class="center">

布局阶段的输出结果称为 “盒模型”（box model）。盒模型精确表达了窗口中每个元素的位置和大小，而且所有的相对的度量单位都被转化成了屏幕上的绝对像素位置。

在最后阶段，已经知道了哪些节点是可视的、它们的样式和它们的几何外观，我们终于能够将这些信息渲染为屏幕上每个真实的像素点了。这个阶段称为“绘制”，或者“格栅化”（rasterizing）。

到这里，各位读者理解了没？上述每一步中浏览器都需要进行大量的计算工作，也就是说需要消耗相当的时间。幸好，Chrome DevTools工具能够帮我们深入了解上述每一步。让我们一起看一看之前例子中“hello world”页面在布局阶段的数据：

<img src="images/layout-timeline.png" alt="Measuring layout in DevTools" class="center">

* 布局阶段的时间轴中捕获了渲染树的构建，以及元素位置和尺寸的计算过程。
* 一旦布局阶段完成，浏览器便开始绘制阶段，将渲染树转化为屏幕上的每个像素点。

渲染树的构建、页面布局以及绘制所花费的时间取决于页面的尺寸、样式的规则以及运行的设备。页面越大，浏览器的工作就越多；规则越复杂，就需要更多的时间来绘制（比如绘制单色比较方便，相比之下阴影的计算和渲染就麻烦的多了）。

一旦上述步骤都完成了，我们的页面就会呈现在屏幕上——噢耶！

<img src="images/device-dom-small.png" alt="Rendered Hello World page" class="center">

我们来快速回顾一下浏览器处理过程中的每一个步骤：

1. 处理HTML标记，生成DOM树
1. 处理CSS标记，生成CSSOM树
1. 将DOM树和CSSOM树合并为渲染树
1. 对渲染树中的内容进行布局，计算每个节点的几何外观
1. 将渲染树中的每个节点绘制到屏幕中

我们的例子页面也许看起来很简单，但是也需要浏览器做一系列计算来呈现出来。读者们觉得当我们修改DOM和CSSOM的时候会发生什么呢？浏览器需要重复上述步骤来确定哪些像素点需要重新进行渲染

**优化关键渲染路径就是减少上述步骤1至步骤5所花费的总时间。** 如此，我们才能尽快将内容渲染到屏幕上，缩短初次加载之后每次刷新的时间间隔，以更快的速度展现页面中的交互式内容。

{% include modules/nextarticle.liquid %}

{% endwrap%}
