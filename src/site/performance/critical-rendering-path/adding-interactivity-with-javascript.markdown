---
layout: article
title: "用Javascript增添交互"
description: "使用Javascript我们能够修改页面的各个方面：内容、样式和用户交互行为。但是，Javascript也可能阻碍DOM结构的生成并延缓页面呈现。使你的Javascript脚本异步加载，并且从关键渲染路径减少任何不必要的脚本以提供最佳性能。"
introduction: "使用Javascript我们能够修改页面的各个方面：内容、样式和用户交互行为。但是，Javascript也可能阻碍DOM结构的生成并延缓页面呈现。使你的Javascript脚本异步加载，并且从关键渲染路径减少任何不必要的脚本以提供最佳性能。"
article:
  written_on: 2014-01-01
  updated_on: 2014-01-05
  order: 4
collection: critical-rendering-path
key-takeaways:
  adding-interactivity:
    - JavaScript能查找以及修改DOM和CSSOM
    - JavaScript执行对CSSOM的阻碍
    - JavaScript会阻碍DOM生成除非明确声明为异步加载
---
{% wrap content %}

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

{% include modules/toc.liquid %}

{% include modules/takeaway.liquid list=page.key-takeaways.adding-interactivity %}

浏览器中运行的JavaScript是一种动态语言，这能让我们更改页面运行的各个方面：我们可以在DOM树上添加或删除元素修改页面内容,我们可以修改CSSOM中每个元素的属性,我们可以处理用户输入,等等。为了说明这一点,让我们继续讨论我们的前面的“Hello World”示例，用一个简单的内联脚本：

{% include_code _code/script.html full %}

* JavaScript让我们能得到DOM，并拉取到与之相关的隐藏的span节点—这个节点在渲染树上可能看不见，但它还是存在于DOM中的！然后，一旦得到了这个节点的引用，我们就可以修改它的文本（通过textContent），甚至覆盖它之前计算好的display属性，从’none’到’inline’。一旦所有的都完成了，页面将会显示 "**Hello interactive students!**".

* JavaScript也能让我们创建样式，在DOM中添加或移除元素。实际上，从技术上来说整个页面都可以写成一个大的JavaScript文件，它会一个接一个地创建元素并设计元素的样式 — 那样也能够做到，但是在实际中用HTML和CSS来做更容易一些。在JavaScript方法中的第二部分，我们创建了一个新的div元素，设置了它的文本内容，赋予了它样式，并且把它添加到了body中。

<img src="images/device-js-small.png" class="center" alt="page preview">

那样做以后，我们修改了一个已存在的DOM节点的内容和CSS样式，并添加了一个全新的节点到文本中。我们的页面不会赢得任何设计奖，但是它说明了JavaScript提供给我们的能力和灵活性。

然而，在其表面下潜藏着一个巨大的性能隐患。JavaScript提供给我们许多功能，但是它同时也在怎样和何时渲染页面方面产生了许多额外的限制。

首先，注意一下在上面的例子中，内联的脚本是在页面下部的。为什么？好吧，你应该自己试一下，但是如果把脚本移到span元素的上面，你会注意到脚本会执行失败，并报出警告说它无法在文本中找到任何span元素 — 例如，getElementsByTagName(‘span’)会返回null。这证明了一个重要了特性：脚本是在它被插入到文本中的位置处执行的。当HTML解析器遇到一个脚本标签时，它会暂停对构建DOM的处理，并让出控制权给JavaScript引擎。一旦JavaScript引擎完成了运行，浏览器再从之前中断的地方重新开始，并继续进行DOM构建。

换句话说，脚本块找不到任何页面后面的元素，因为他们还没有被创建！或者，略微不同地表达一下：**执行内联脚本会阻塞DOM的构建，这也会延迟页面的初次渲染。**

把脚本引入到页面中的另一个巧妙的好处是，它们可以读取和修改的不仅仅是DOM，还有CSSOM属性。实际上，那正是我们在例子中所做的，我们把span元素的显示属性从none改变为inline。最后的结果是什么？我们现在有了一个竞争条件。

假如浏览器在我们运行脚本时还没有完成CSSOM的下载和创建会怎么样？答案很简单，并且对于性能不会很好：**浏览器会延迟脚本的执行，直到它完成了CSSOM的下载和构建，当我们在等待时，DOM的构建也被阻塞了！**

简单来说，JavaScript引入了许多DOM、CSSDOM和JavaScript执行之间的依赖，并且在浏览器如何快速处理和渲染页面到屏幕上时产生了严重的延迟：

1. 脚本在文本中的位置是很关键的
1. 当遇到script标签时DOM的构建会被暂停，直到脚本完成了执行
1. JavaScript可以查询和修改DOM和CSSOM
1. 直到CSSOM准备好了，JavaScript才会执行

当我们讨论“优化关键渲染路径”时，更大程度上是在讨论理解和优化HTML、CSS和JavaScript之间的依赖关系。


## 阻塞解析器和异步的JavaScript

在默认情况下，JavaScript的执行是会“阻塞解析器”的：当浏览器在文本中遇到一个script时，它必须停止DOM的构建，让出控制权给JavaScript来运行，在继续构建DOM之前让脚本来执行。我们已经在前面的例子中使用了一个内联的脚本的行动中看到了这个。实际上，内联的脚本通常是会阻塞解析器的，除非你特别的进行了处理，写了一些额外的代码来延迟它们的执行。

通过script标签引入的脚本是怎样的呢？拿之前的例子来说，把代码抽出来放到一个单独的文件里：

{% include_code _code/split_script.html full %}

**app.js**

{% include_code _code/app.js full javascript %}

当使用`<script>`标签而不是使用内联的JavaScript片段时，你认为执行顺序会不同吗？当然，答案是“no”，因为他们是一样的，并且以相同的方式表现。在这两种情况下，浏览器都会在它能够处理余下的文本之前，暂停并执行脚本。然而， **在使用外部的JavaScript文件的情况下，浏览器将不得不暂停解析，等待从磁盘、缓存或远程服务器上获取这个脚本，这会给关键渲染路径增加数十到数千毫秒的延迟。**

从另一个角度来看，这也是一个好消息，给我们带来另一种解决方案！默认情况下，JavaScript是会阻塞解析器的，浏览器并不知道脚本计划在页面上做什么，因此它不得不假定最坏的情况，并阻塞解析器。然而，如果我们可以给浏览器一个标识呢，告诉它脚本不必在它被引入到文件里的位置处执行。这样做就可以让浏览器继续构建DOM，让脚本在它准好以后再执行 — 例如，文件已经从缓存或远程服务器上获取到。

那么，我们怎样来实现这一方法呢？相当的简单，我们可以把脚本标记为async：

{% include_code _code/split_script_async.html full %}

添加async关键词到script标签中，在浏览器等待脚本变得可用时，告诉它不必阻塞DOM的构建 — 这是在性能上是一个巨大的优势！

{% include modules/nextarticle.liquid %}

{% endwrap %}
