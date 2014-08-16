---
layout: article
title: "创建对象模型"
description: "浏览器在渲染页面之前，需要创建DOM和CSSOM树。因此我们应该确保尽快将HTML和CSS交给浏览器。"
introduction: "浏览器在渲染页面之前，需要创建DOM和CSSOM树。因此我们应该确保尽快将HTML和CSS交给浏览器。"
article:
  written_on: 2014-04-01
  updated_on: 2014-04-28
  order: 1
collection: critical-rendering-path
key-takeaways:
  construct-object-model:
    - 字节（Bytes) -> 字符(characters) -> 标记(tokens) -> 节点(nodes) -> 对象模型
    - HTML标记被转换成文档对象模型（DOM)，CSS标记被转换成CSS对象模型（CSSOM)
    - DOM和CSSOM是独立的数据结构
    - Chrome DevTools Timeline工具能让我们观察DOM和CSSOM的创建过程以及处理过程的消耗
notes:
  devtools:
    - We'll assume that you have basic familiarity with Chrome DevTools - i.e. you know how to capture a network waterfall, or record a timeline. If you need a quick refresher, check out the <a href="https://developers.google.com/chrome-developer-tools/">Chrome Developer Tools documentation</a>, or if you're new to DevTools, I recommend taking the Codeschool <a href="http://discover-devtools.codeschool.com/">Discover DevTools</a> course.
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

{% include modules/toc.liquid %}

{% include modules/takeaway.liquid list=page.key-takeaways.construct-object-model %}

## 文档对象模型（DOM)

{% include_code _code/basic_dom.html full %}

我们开始吧! 很简单的一个纯HTML页面，只有一些文本和一张图。在渲染这样一个简单页面的过程中，浏览器需要做些什么？

<img src="images/full-process.png" alt="DOM construction process">

1. **转换：** 浏览器从磁盘或者网络上读取HTML的原始字节，然后根据指定的编码规则转换成单独的字符（比如按UTF-8编码）。
1. **标记分解：** 浏览器将字符串按照[W3C HTML5 标准](http://www.w3.org/TR/html5/)标准转换成确定的标记，比如、以及其他带尖括号的字符。每个标记都有特定的意义以及一套规则。
1. **词法：** 分解出来的标记被转换成能定义其属性和规则的对象。
1. **DOM构造：** 最终由于HTML标记定义了不同标签的关系（有些标签嵌套在其他标签里面），创建出来的对象被关联到一个树形数据结构。这颗树会反映在原先标签里定义的父子关系，比如HTML对象就是body对象的父对象，body对象又是paragraph对象的父对象等等。

<img src="images/dom-tree.png" class="center" alt="DOM tree">

**整个过程的最终输出就是文档对象模型，或者是我们简单页面的DOM，浏览器将用于对网页作进一步处理。**

浏览器每次处理HTML标记都会经过上面的步骤：将字节转换成字符，识别标记，将标记转换成节点，然后创建DOM树。整个过程可能会消耗一定的时间，尤其是处理大量HTML的时候。

<img src="images/dom-timeline.png" class="center" alt="Tracing DOM construction in DevTools">

{% include modules/remember.liquid title="Note" list=page.notes.devtools %}

如果你打开Chrome DevTools，在页面加载过程中录制一个时间轴，你就可以看到完成上面的步骤花费的实际时间。对于上文的例子，从HTML字节到DOM树，一共花了大约5ms。当然了，如果页面更大，事实上大部分页面都比示例要大，那么这个过程可能也会显著花掉更多时间。在后面关于创建平滑动画的章节中你会发现当浏览器需要处理大量HTML的时候这很可能会成为瓶颈。

DOM树创建好了，是不是就表示获得了足够的信息，可以渲染页面了呢？还不是，DOM树包含有文档标记的关系和属性信息，但它不能告诉我们元素渲染之后应该长什么样。这就是CSSOM的责任了，下面就要介绍它

## CSS对象模型（CSSOM）

浏览器创建完DOM后，在head区间遇到一个link标签，它引用了一个外部的CSS样式表：style.css。由于预计到可能需要这个资源来渲染页面，浏览器立即发出了一个资源请求，返回的内容如下：

{% include_code _code/style.css full css %}

当然我们可以在HTML标签里直接声明样式（内联），但将CSS独立出去可以让我们区别对待内容和显示：设计师可以专注CSS，开发人员专注HTML等等。

与HTML一样，我们也需要将获得的CSS规则转换成浏览器能够理解并执行的东西。所有我们再一次重复了一个与HTML非常相似的过程：

<img src="images/cssom-construction.png" class="center" alt="CSSOM construction steps">

CSS字节被转换成字符然后转换成标记和节点，最终被关联到一个被称为”CSS对象模型“或者简称为CSSOM的树形结构中。

<img src="images/cssom-tree.png" class="center" alt="CSSOM tree">

为什么CSSOM要采用树作为数据结构？ 在计算任何对象的最终样式的时候，浏览器从适用于这个节点的最宽泛的规则开始（比如对于一个body元素的子元素，所有的body样式都适用于它），然后采用更具体的样式来递归的修改计算出的样式。

为了具体说明我们来看上面的CSSOM树。 放在body元素里面的span标签，它里面包含的任何文字都具有16像素的大小，颜色为红色， font-size规则会从body向下级联到span。然而如果span是段落(p)元素的子元素，则它的内容不会显示。

注意上面的树并不是一个完整的CSSOM树，它只显示了我们决定要在样式表里覆盖的部分。 每个浏览器都提供了一套默认的样式，被称为”用户代理样式“，它就是当你不提供任何自定义样式时看到的样式，我们的自定义样式也只是在默认样式的基础上进行覆盖 (e.g. [默认 IE 样式](http://www.iecss.com/))。 如果你再Chrome DevTools查看过计算后的样式，又搞不懂这些样式从哪儿来，现在你应该知道答案了。

想知道CSS处理花了多少时间吗？在DevTools里录制一个时间轴然后查找”样式再计算”事件（Recalcuate Style）， 与DOM解析不同，时间轴不会单独显示”处理CSS“条目，我们需要捕获”解析“和CSSOM树创建，并加上这个事件下面遍历计算样式的时间。

<img src="images/cssom-timeline.png" class="center" alt="Tracing CSSOM construction in DevTools">

我们的微型样式表需要大约0.6ms进行处理，最终影响到页面上8个元素的显示。耗时虽然不长但不是没有。 但这8个元素从哪儿来？CSSOM和DOM是独立的数据结构。原来，浏览器掩藏了重要的一步。接下来就要讲到将DOM和CSSOM关联的渲染树。

{% include modules/nextarticle.liquid %}

{% endwrap %}
