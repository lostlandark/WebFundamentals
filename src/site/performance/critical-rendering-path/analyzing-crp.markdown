---
layout: article
title: "分析关键渲染路径的性能"
description: "识别和解决关键渲染路径性能的瓶颈，需要对一些常见的陷阱有所了解。我们来具体实践一下，提取出一些通用的性能模式，以帮助你优化页面。"
introduction: "识别和解决关键渲染路径性能的瓶颈，需要对一些常见的陷阱有所了解。我们来具体实践一下，提取出一些通用的性能模式，以帮助你优化页面。"
article:
  written_on: 2014-04-01
  updated_on: 2014-04-28
  order: 6
collection: critical-rendering-path
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

优化渲染路径的目的是为了让浏览器能够以最快速度渲染页面——更快的页面渲染可以得到更高的参与和更多的页面浏览量，同时也能[提高转化率](http://www.google.com/think/multiscreen/success.html)。因此，我们需要通过优化资源加载及其加载顺序来减少用户不得不盯着空白屏的时间。

为了方便说明其中的过程，让我们先从简单的例子做起，一步一步添加其他资源、样式和程序逻辑。在这个过程当中，我们也可以了解一些会出现问题的地方，并知道每种情况的优化方案。

最后，在开始之前，我们还有一个事情要说明。。到目前为止，我们一直将思路集中于当资源（CSS,JS或HTML文件）都是直接可用的并且忽略资源从服务器上或缓存中获取的时间的情况下浏览器发生了什么。我们会在下一节详细说明如何优化网络方面的东西，在这一节，我们假定：

* 服务器的网络延迟耗时100毫秒
* 服务器返回HTML文件耗时100毫秒，其他所有文件耗时10毫秒。

## Hello World实践

{% include_code _code/basic_dom_nostyle.html full %}

我们从最基本的HTML标签和一张简单的图片开始，没有CSS和Javascript文件，这个步骤非常简单。现在让我们打开谷歌调试工具的Network timeline，然后查看资源加载流：

<img src="images/waterfall-dom.png" alt="" class="center" alt="CRP">

结果如同预期，花费了大约200毫秒的时间去下载HTML文件。注意,蓝色线的透明部分表示浏览器等待网络的时间，即这段时间没有收到服务器端的返回数据。而不透明的部分表示下载完第一个字节数据的时间点。在上边的例子中，HTML文件的加载是非常小的（小于4K），所以我们需要一个往返就可以获取完整的文件。因此，HTML文件花费了大约200毫秒的时间去下载，其中一半的时间花费在网络等待上，另一半时间则耗费在服务器返回上。

当HTML文档状态变为可用的时候，浏览器开始解析这些数据，将这些数据转化为标签，并开始构建DOM树。注意看开发者工具下方关于DOMContentLoaded事件触发时间的提示（216毫秒），这个提示跟蓝色的线是相对应的。HTML下载完成后与蓝色垂直线（DOMContentLoaded事件）之间的空白部分是浏览器构建DOM树的时间，在这个例子中，用时仅几毫秒。

最后，有个有趣的事情，那就是我们的「awesome photo」并没有阻塞domContentLoaded事件！原来，我们可以在不等待每个页面资源的情况下构建渲染树，甚至渲染页面：**对于首次快速渲染来说，不是所有资源都是必要的**。实际上正如我们所见，我们在谈论关键渲染路径的时候，我们实际上是在谈论HTML标签，CSS和Javascript。图片并不会阻塞最初的页面渲染——不过当然，我们也应该尽力去确保图片加载尽量的快！

话虽如此，”load”事件（通常叫”onload”事件）是被图片阻塞住了，开发者工具在335毫秒的时候报告了onload事件。回忆一下，当所有需要加载的资源已经被页面下载和处理的时候，onload事件标记了一个点，这个点是浏览器页签上的圆圈停止的时间，在加载流中，这个点是用红色的垂直线标记的。


## 加入CSS和Javascript

表面上来看，我们的「Hello world 初体验」页面也许很简单，但是底层做了很多事才让上述的一切发生！话虽如此，在实践中我们需要比HTML更多的东西——我们将把一个CSS样式表和一个或多个脚本文件添加到页面上以增加一些交互性。让我们把它们加进来，看看会发生什么：

{% include_code _code/measure_crp_timing.html full %}

_添加Javascript和CSS之前：_

<img src="images/waterfall-dom.png" alt="DOM CRP" class="center">

_添加Javascript和CSS之后：_

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" class="center">

添加了外部CSS和Javascript文件也就在我们的加载流中添加了两个额外的请求，所有这些在大概同一时间被浏览器调用了，目前为止一片祥和。然而需要注意的是， **domContentLoaded事件和onload事件的时间差更小了。发生了什么？**

* 与之前简单的HTML例子不同，现在我们需要获取和解析CSS文件去构建CSSOM，并且我们同时需要DOM和CSSOM去构建渲染树。
* 因为我们还有一个解析器阻塞我们的页面上的JavaScript文件。直到CSS文件被下载和解析之前，domContentLoaded事件都是被阻塞的，原因是：也许Javascript需要查询CSSOM，因此在我们可以执行Javascript之前必须阻塞并等待CSS文件。

**如果我们将外部脚本文件换成一个行内脚本呢？** 一个表面上微不足道的问题其实非常棘手。事实证明，就算脚本直接内联在了页面中，对于浏览器来说，想知道脚本会做什么的唯一靠谱的方式。。就是执行它。正如我们知道的，直到CSSOM被构造之前我们都无法执行它。长话短说，解析器也会阻塞内联的Javascript。

话虽如此，就算被CSS阻塞了，内联的脚本能使页面加载更快吗？如果最后一个方案是棘手的，那么这个方案就更是如此！让我们试一下看看究竟会发生什么。

_外部Javascript:_

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" class="center">

_内部Javascript:_

<img src="images/waterfall-dom-css-js-inline.png" alt="DOM, CSSOM, and inlined JS" class="center">

我们减少了一个请求，但是onload事件和domContentLoaded事件所用的时间基本是相同的。为什么呢？现在我们知道，Javascript是内联的还是外部引用的都不重要，因为只要浏览器命中script标签，就会阻塞并等待CSSOM构造完毕。此外，在我们的第一个例子中，CSS文件和Javascript文件被浏览器并行下载的时间都差不多。因此，在这个特定的情况下，内联的Javascript并没有起到什么帮助！恩。。所以现在我们被卡住了。难道在让我们的页面渲染更快的事情上我们无能为力了么？事实上，我们有几种不同的策略。
首先，回想一下，解析器会阻塞所有的内联脚本，但是通过给外部引入的脚本的标签上加上”async”属性，可以使解析器不阻塞。让我们取消内联脚本，然后尝试一下：

{% include_code _code/measure_crp_async.html full %}

_解析器阻塞（外部引入）的Javascript:_

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" class="center">

_异步的（外部引入）的Javascript:_

<img src="images/waterfall-dom-css-js-async.png" alt="DOM, CSSOM, async JS" class="center">

已经好多了！domContentLoaded事件在HTML被解析之后就被触发了：浏览器知道不去阻塞Javascript，并且因为没有其他的解析器阻塞脚本，CSSOM构造也可以同步的进行了。

或者我们可以尝试不同的方法，同时内联CSS和Javascript：

{% include_code _code/measure_crp_inlined.html full %}

<img src="images/waterfall-dom-css-inline-js-inline.png" alt="DOM, inline CSS, inline JS" class="center">

注意，domContentLoaded事件耗费的时间实际上与前一个例子是一样的——代替给Javascript加上async属性的方案，我们已经将CSS和JS内联到页面上。这让我们的HTML页面更长了，但这一方案的好处是不用花时间去等待浏览器加载外部资源了，一切都在页面中。

正如我们所见，就算是一个非常简单的页面，优化关键渲染路径也不是个简单的事情。我们需要了解不同资源之间的依赖关系图，我们也需要确定哪些资源是”必不可少”的，并且，我们必须对如何引入这些页面资源选择不同的策略。没有一个完美的解决方案，因为每个页面都是不同的，并且你必须自己独立的遵循类似的过程去找到最佳优化策略。

说到这里，我们看看能不能退一步，找到一些常规的性能模式。


## 性能模式

最简单的页面由HTML标签组成，没有CSS，没有JavaScript或其他类型的资源。渲染这样的页面浏览器需要发请求然后等待HTML文档的返回，解析它，构建DOM最后将它渲染到页面上：

{% include_code _code/basic_dom_nostyle.html full %}

<img src="images/analysis-dom.png" alt="Hello world CRP" class="center">

**T<sub>0</sub> 到 T<sub>1</sub> 的时间捕获了网络和服务器的处理时间。** 最好的情况下（如果HTML文件很小），我们在网络中只需要一个往返就能获取全部的文档。由于TCP传输协议的工作原理，越大的文件可能会需要更多的网络往返次数，这个主题我们会在以后的课中讲到。 **总的来说，我们可以说上面的网页在做好的情况下有一个往返（最小）关键渲染路径。**

现在，让我们思考在加入了外部CSS文件的相同页面下的情况：

{% include_code _code/analysis_with_css.html full %}

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" class="center">

再一次的，我们为了获取HTML文档引发了一次网络往返，并且检索标签告诉告诉我们，我们将同时需要CSS文件，这意味着浏览器能够在屏幕上渲染页面之前，需要回到服务器端去获取CSS文件。那么结论是，在这个页面能够显示出来之前，要进行最少两次往返。再说一次，CSS文件有可能需要多个往返，因此要强调”最小值”。

我们来定义一下我们将要用来描述关键渲染路径的词汇：

* **关键资源：** 可能会阻塞页面最初渲染的资源。
* **关键路径长度：** 往返的次数，或者加载所有关键资源的时间总和。
* **关键字节：** 页面首次渲染需要请求的字节数量，即所有关键资源文件大小总和。 我们第一个简单的html页面的例子包含了一个单独的关键资源（即HTML文档），关键路径长度也等于一个网络往返（假设文件很小），关键字节总数就等于传输的HTML文档自身的大小。

现在，让我们来比较上面按个HTML+CSS的例子的关键路径的特征：

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" class="center">

* **2** 个关键资源
* **2** 或2个以上往返的最小关键路径长度
* **9** KB关键字节

构造渲染树同时需要HTML和CSS文件，因此HTML和CSS都是关键资源：CSS只在浏览器得到HTML文档后被获取，因此关键路径长度的最小值是两个往返；两个文件加载一起共有9KB的关键字节数。

好的，让我们把外部引入的Javascript也加进来！

{% include_code _code/analysis_with_css_js.html full %}

我们添加了app.js，这是页面上的一个外部引入的Javascript资源。并且现在我们知道，这是个解析器阻塞（即关键）资源。更糟糕的是，为了执行Javascript文件，我们不得不阻塞并等待CSSOM-回想一下，Javascript可以查询CSSOM，因此浏览器直到”style.css”下载完并且CSSOM构建之前都将暂停。

<img src="images/analysis-dom-css-js.png" alt="DOM, CSSOM, JavaScript CRP" class="center">

话虽如此，在实践中如果我们去看页面的”网络流”，就会发现CSS和Javascript请求将会在大约同一时间被发出。浏览器获取到HTML，发现了两个资源，然后发起了两个请求。因此，上面的页面有以下关键路径特征：

* **3** 个关键资源
* **2** 或2个以上往返的最小关键路径长度
* **11** KB关键字节

我们现在有三个关键资源，它们相加起来有11KB的关键字节，但是我们的关键路径长度始终是两个往返，因为加载CSS和JS是并行的！弄清楚关键渲染路径的特征意味着能够弄清楚什么资源是关键资源，也能明白浏览器是如何安排他们的获取的。让我们继续我们的例子…

在于我们的网站开发人员聊完之后，我意识到我们包含在页面中的Javascript不需要被阻塞：我们有一些不需要阻塞我们页面渲染的分析代码和其他代码，为了不阻塞解析器，可以在script标签上加上”async”属性。

{% include_code _code/analysis_with_css_js_async.html full %}

<img src="images/analysis-dom-css-js-async.png" alt="DOM, CSSOM, async JavaScript CRP" class="center">

使用异步脚本有几个优势：

* 脚本不再解析器阻塞，并且再不是关键渲染路径的一部分了。
* 因为没有其他关键脚本，CSS也可以不用阻塞domContentLoaded事件了。
* domContentLoaded事件触发的越早，应用逻辑被执行的就越早。

结果就是，我们优化过的界面现在回到了两个关键资源（HTML和CSS），两个往返的最小关键路径长度和9KB的关键字节数总和。最后，我们说CSS样式只被打印所需要？看起来如何？

{% include_code _code/analysis_with_css_nb_js_async.html full %}

<img src="images/analysis-dom-css-nb-js-async.png" alt="DOM, non-blocking CSS, and async JavaScript CRP" class="center">

因为style.css文件只用作打印使用，浏览器不需要在渲染页面的时候被它阻塞。因此，当DOM构建完毕。浏览器已经有了足够的信息去渲染页面！因此，这个页面只有一个关键资源（HTML文档）和一个往返的最小关键渲染路径长度。

{% include modules/nextarticle.liquid %}

{% endwrap%}
