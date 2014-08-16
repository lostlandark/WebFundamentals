---
layout: article
title: "关键渲染路径"
description: "优化关键渲染路径对于提高性能至关重要：我们的目标是将用户需要的信息优先化并且展现在用户面前，从而帮助用户来完成他的首要行动。"
introduction: "优化关键渲染路径对于提高性能至关重要：我们的目标是将用户需要的信息优先化并且展现在用户面前，从而帮助用户来完成他的首要行动。"
article:
  written_on: 2014-04-01
  updated_on: 2014-04-28
  order: 1
id: critical-rendering-path
collection: performance
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

传递一个快速的网络体验需要浏览器来做大量的工作。大多数作业都是在开发者看不到的情况下发生的。但是浏览器是如何消化我们所提供的HTML, CSS, 与Javascript 到渲染出屏幕上的像素？

优化性能完全基于对于浏览器处理文件步骤的理解， 通过对于HTML，CSS, 还有Javascript的控制来将他们变成像素 - 这就是所谓的**关键路径渲染**。

<img src="images/progressive-rendering.png" class="center" alt="progressive page rendering">

通过对于关键路径的渲染，我们可以大幅度的降低第一次读取网页的速度。另外，对于关键渲染路径的理解对于建立一个正常运转，拥有互动性的网络应用至关重要。巧的是，对于互动性的优化大同小异，只需要在60帧每秒的情况下完成一个完整的循环！但是，我们先不要急功近利。第一步，我们来一个快速，基础的概述来理解浏览器是如何展示一个网页的。

## 课程

{% for guide in page.articles.critical-rendering-path %}
1. [{{guide.title}}]({{site.baseurl}}{{guide.url | clean}}) &mdash;
{{guide.description}}
{% endfor %}

{% endwrap%}
