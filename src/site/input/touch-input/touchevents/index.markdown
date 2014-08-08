---
layout: article
title: "实现自定义手势"
description: "使用触摸事件，如此一来你可以为你的用户界面创造自定义交互方式。但是问题是，你如何能有效率地做到这件事情，以保持高帧数和用户的愉悦。"
introduction: "如果你有为你的网站添加自定义交互方式和手势的想法，那么有两点需要谨记，一，如何支持不同种类的移动浏览器；二，如何保持高帧数。在此篇中，我们将窥探一下关于这方面的内容。"
article:
  written_on: 2014-01-01
  updated_on: 2014-01-06
  order: 2
rel:
  gplusauthor: https://plus.google.com/+MattGaunt
key-takeaways:
  touch-events:
    - 对于全设备的支持，管理触摸，鼠标以及指针事件。
    - 时刻为元素本身绑定起始监听器。
    - 如果你想让用户与某一特定元素进行交互，那么用touchstar方法为文档绑定你的移动和结束监听器。请确保在结束监听时将他们从文档中解绑。
    - 如果你想支持多点触控，那么你可以将移动和结束的触摸事件绑定到元素本身，或者在一个元素上控制所有触摸。
remember:
  touch-action:
    - 使用 <code>touch-action&colon; pan-x</code> 或者
      <code>touch-action&colon; pan-y</code> 能够很明确地将你的意图传递给用户，即用户应只在一个元素上垂直或者水平滚动。
collection: touch-input
---

{% wrap content%}

<!-- TODO[MATTGAUNT] add related items -->
<!-- TODO[MATTGAUNT] add what's next -->

{% include modules/toc.liquid %}

## 用事件来响应触控输入

这取决于你使用触摸做什么，你可能会遇到以下两种状况之一：

- 我想让用户与某一特定元素进行交互。
- 我想让用户同时与多种元素进行交互。

这两种交互都有取舍之处。

如果用户将只能够和一个元素进行交互，你可能想：只要手势最初从某个元素开始，就让全部的触摸事件都集中在该元素上。例如：将手指从可滑动的元素上离开时却仍能操控这个元素。

![Example GIF of touch on document](images/touch-document-level.gif)

然而，如果你期待用户同时与多元素进行交互（使用多点触控），那么你应该对特定元素进行触控限制。

![Example GIF of touch on element](images/touch-element-level.gif)

{% include modules/takeaway.liquid list=page.key-takeaways.touch-events %}

### 添加事件监听器

触摸事件和鼠标事件在大多数移动浏览器上能够执行。

你所需要执行的事件名称是`touchstart`, `touchmove`, `tauchend`和`touchcancel`。

在一些情况下，你可能发现你同时想支持鼠标交互，那么你可以使用鼠标事件：
`mousedown`， `mousemove`， 和 `mouseup`。

对于Windows触控设备，你需要支持一系列新事件当中的指针事件。指针事件将鼠标事件和触摸事件合并在了同一回调系列中。目前，指针事件和`MSPointerDown`事件，`MSPointerMove`事件，以及`MSPointerUp`事件只支持Internet Explorer 10和更高版本。

触控，鼠标和指针事件是为你的程序添加手势操作的砖瓦。 （请参考 [触摸，鼠标和MS指针事件](#touch-mouse-and-ms-pointer-events)).

将这些事件及其回调函数与布尔值放置在`addEventListener()`方法中。这个布尔值决定了其他元素有机会捕获并执行该事件之前或之后，你是否能够捕获这个事件。（真值`ture`意味着我们想要这个事件放在其他元素之前执行）：

{% include_code ../_code/touch-demo-1.html addlisteners javascript %}

这些代码首先使用`window.navigator.msPointerEnabled`检测是否支持指针事件。若不支持，我们就换用触控事件和鼠标事件监听器。

`window.PointerEventSupport`的值是检测`window.PointerEvent`或现已被废弃的`window.navigator.msPointerEnabled`对象是否存在。如果它支持，我们就可以用事件名的变量，由`window.PointerEvent`是否存在决定是否使用版本前缀。

### 控制单个元素交互

在上方简短的几行代码中你可能已经注意到了我们只添加了开始事件监听器，这是我们有意为之。

在触摸手势开始的时候，在元素上添加移动和结束事件监听器，可以让浏览器检测触摸事件是否发生在事件监听器范围之内，倘若不是，也可以通过避免运行额外的javascript来加快运行速度 。

![Illustrating Binding Touch Events to Document in touchstart](images/scroll-bottleneck.gif)

以下为执行中所需的步骤：

1. 为某一元素添加开始事件监听器。
1. 在你的触控开始方法中，为文档绑定移动和结束元素。其原因是我们可以因此无视事件是否发生在原始元素上而接收所有事件。
1. 控制移动元素。
1. 在结束事件中，为文件移除移动监听器和结束监听器。

以下是我们`handleGestureStart`方法的部分代码，其中为文档添加了移动和结束事件：

{% include_code ../_code/touch-demo-1.html handle-start-gesture javascript %}

我们添加的结束回调是`handleGestureEnd`，它会在手势完成后，从文件中移除移动事件和结束事件：

{% include_code ../_code/touch-demo-1.html handle-end-gesture javascript %}

鼠标事件的模式也是一样的，因为对用户来说，他们偶尔非常容易点击非元素所在的地方。这导致了移动事件失效。通过为文档添加移动事件，不管在界面的何处，我们都可以持续获得鼠标移动的信息。

你可以在Chrome DevTools中使用`"Show potential scroll bottlenecks"`功能，展示触摸事件的工作状态：

<img class="g-medium--full g-wide--full" src="images/scroll-bottleneck-devtool.png" alt="Enable Scroll Bottleneck in DevTools">

通过这个方法，你可以看到触摸事件绑定到了哪，并确认你的添加与移除监听器逻辑是否如你所愿地工作。

<div style="clear: both;"></div>

### 控制多元素交互

如果你希望你的用户一次使用多个元素，你可以直接为元素添加移动事件监听器和结束事件监听器。这只应用于触控，因为你应该继续将`mousemove`以及`mouseup`监听器应用于文档。

因为我们只希望追踪某一特定元素上的触控，所以我们能够马上为元素的触摸及指针事件添加移动和结束监听器：

{% include_code ../_code/touch-demo-2.html addlisteners javascript %}

在我们的`handleGestureStart`和`handleGestureEnd`函数中，我们分别为文档添加和移除了鼠标事件监听器。

{% include_code ../_code/touch-demo-2.html handle-gestures javascript %}

## 使用触摸时是60fps

现在，我们拥有了所关心的开始事件和结束事件，因此我们能够真正地给予触摸事件以回应。

### 获取并储存触摸事件坐标

对于任何一个开始事件和移动事件，你都可以轻松从某一事件中提取`x`和`y`。

以下的代码片段通过查询`targetTouches`来检查事件是否来自触摸事件。如果是，稍后会从首次触摸中提取`clientX`和`clientY`。如果事件是一个鼠标事件或者是指针事件，稍后我们就直接从事件本身里提取`clientX`和`clientY`。

{%include_code ../_code/touch-demo-2.html extract-xy javascript %}

所有的触摸事件的触摸数据中都有三个列表
（亦可参考 ［触摸列表］（#touch-lists））：

* `touches`：屏幕上当前所有触摸的列表，忽略正在运行的DOM元素。
* `targetTouches`：当前在被绑定了事件的DOM元素上的触摸事件列表。
* `changedTouches`：发生改变并导致事件处于激活状态的触摸事件列表。

在大部分情况下，`targetTouches`可以提供你所需要的一切。

### 请求动画帧数

因为事件回调在主线程中被激活，我们会想要在回调中尽可能地运行一部分代码，以保持框架的高帧率并防止遗漏。

使用`requestAnimationFrame`来改变用户界面以响应某一事件。在浏览器计划构建一个框架的时候，这给予你一次升级用户界面的机会。并会帮助你解决一些回调问题。

典型的执行步骤是从开始事件和移动事件中保存`x`和`y`的坐标，然后在移动事件回调中获取一个动画帧animation frame。

在我们的演示中，我们将初始触摸位置保存在`handleGestureStart`中：

{% include_code ../_code/touch-demo-1.html handle-start-gesture javascript %}

如果我们需要的话，`handleGestureMove`方法可以在获取动画框架前保存`y`的位置，
并作为回调传送给我们的`onAnimframe`函数：

{%include_code ../_code/touch-demo-2.html handle-move javascript %}

在`onAnimFrame`函数中，我们得以改变用户界面以移动元素。首先，我们要检查手势是否任在运行，再决定我们是否要添加动画特效。如果是那样，我们使用y的初始位置和结束位置来为我们的元素计算新的变动。

一旦完成变动，我们就将`isAnimating`赋值为`fasle`，这样一来，下一个的触控事件就要获取一个新的动画帧。

{%include_code ../_code/touch-demo-2.html on-anim-frame javascript %}

## 使用触摸动作控制卷动

CSS属性中的`touch-action`允许你在触控时控制滚动操作。在我们的例样中，我们使用`touch-action: none`以禁用触摸时的滚动操作。

{%include_code ../_code/touch-demo-1.html touch-action-example css %}

`touch-action`允许浏览器禁用手势，比如IE10+支持双击缩放，并通过设置`pan-x | pan-y | manipulation`触摸动作防止你的双击行为。

这样做的好处是，它可以允许你自己实现这些手势，但是在IE10+的这种情况下，你也移除了200毫秒的点击延迟。

以下是*touch-action*可用的参数列表。

<table class="table-2">
  <thead>
    <tr>
      <th>属性</th>
      <th>描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Property"><code>touch-action: auto</code></td>
      <td data-th="Description">
        浏览器会根据其支持的触控添加交互。比如x轴滚动、y轴滚动、双指缩放和双击。
      </td>
    </tr>
    <tr>
      <td data-th="Property"><code>touch-action: none</code></td>
      <td data-th="Description">浏览器禁用触摸交互。</td>
    </tr>
    <tr>
      <td data-th="Property"><code>touch-action: pan-x</code></td>
      <td data-th="Description">允许浏览器水平滚动，禁用垂直滚动及手势。</td>
    </tr>
    <tr>
      <td data-th="Property"><code>touch-action: pan-y</code></td>
      <td data-th="Description">允许浏览器垂直滚动，禁用水平滚动及手势。</td>
    </tr>
  <tr>
      <td data-th="Property"><code>touch-action: manipulation</code></td>
      <td data-th="Description">允许浏览器双方向滚动，也允许双指缩放；但忽略其他手势。</td>
    </tr>
  </tbody>
</table>

{% include modules/remember.liquid title="Remember" list=page.remember.touch-action %}

## 参考

权威的触摸事件参考条目可以在这里找到：
[w3触摸事件](http://www.w3.org/TR/touch-events/).

### 触摸，鼠标和MS指针事件

这些事件是为你的程序添加新的手势操作的砖瓦：

<table class="table-2">
  <thead>
    <tr>
      <th>触摸，鼠标和MS指针事件</th>
      <th>描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Event Names">
        <code>touchstart</code>,
        <code>mousedown</code>,
        <code>MSPointerDown</code>
      </td>
      <td data-th="Description">
        在单指首次触摸元素或者当用户点击鼠标时被触发。
      </td>
    </tr>
    <tr>
      <td data-th="Event Names">
        <code>touchmove</code>,
        <code>mousemove</code>,
        <code>MSPointerMove</code>
      </td>
      <td data-th="Description">
        用户在屏幕上移动手指或者使用鼠标拖拽时被触发。
      </td>
    </tr>
    <tr>
      <td data-th="Event Names">
        <code>touchend</code>,
        <code>mouseup</code>,
        <code>MSPointerUp</code>
      </td>
      <td data-th="Description">
        在用户停止触摸和释放鼠标时被触发。
      </td>
    </tr>
    <tr>
      <td data-th="Event Names">
        <code>touchcancel</code>
      </td>
      <td data-th="Description">
        在浏览器撤销触摸手势时被触发。
      </td>
    </tr>
  </tbody>
</table>

### 触摸列表

每个触摸事件包括三个列表属性：

<table class="table-2">
  <thead>
    <tr>
      <th>属性</th>
      <th>描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Attribute"><code>touches</code></td>
      <td data-th="Description">
        目前屏幕上所有触控的列表，无论元素是否被触摸。
      </td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>targetTouches</code></td>
      <td data-th="Description">
        当前事件的目标元素，自该事件开始所接收的所有触控行为的列表。例如：如果你绑定一个<code>&lt;button&gt;</code>，你将只会得到当前在那个按键上的触控。如果你绑定一个文档，你将会得到当前在那个文档上的所有触控。
      </td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>changedTouches</code></td>
      <td data-th="Description">
        发生改变并导致事件处于激活状态的触摸事件列表。
        <ul>
          <li>
            对于<code><a href="http://www.w3.org/TR/touch-events/#dfn-touchstart">touchstart</a></code>
             事件-- 当前事件刚激活的触控点列表。
          </li>
          <li>
            对于<code><a href="http://www.w3.org/TR/touch-events/#dfn-touchmove">touchmove</a></code>
             事件-- 上次事件以来已经发生移动的触控点列表。
          </li>
          <li>
            对于 <code><a href="http://www.w3.org/TR/touch-events/#dfn-touchend">touchend</a></code>
            事件和<code><a href="http://www.w3.org/TR/touch-events/#dfn-touchcancel">touchcancel</a></code>
            事件-- 界面上已被移除的触控点。
          </li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

{% include modules/nextarticle.liquid %}

{% endwrap %}
