---
layout: article
title: "使用 requestAutocomplete API简化结账手续"
description: 尽管`requestAutocomplete`的设计初衷是为了帮助用户填写表单，如今，它在电子商务中最为广泛使用。电子商务中的弃购率<a href='http://seewhy.com/97-shopping-cart-abandonment-rate-mobile-devices-concern-you/'>高达97%</a>。
introduction: "`requestAutocomplete`的设计初衷是为了帮助用户填写表单，如今，它在电子商务中最为广泛使用。电子商务中的弃购率<a href='http://seewhy.com/97-shopping-cart-abandonment-rate-mobile-devices-concern-you/'>高达97%</a>。 想象一下，在一个超市里，97%的人得购物车里装满了他们想要的各种东西，但是最后随手一放，然后离开了超市。"
article:
  written_on: 2014-04-30
  updated_on: 2014-04-30
  order: 4
rel:
  gplusauthor: https://plus.google.com/+PeteLePage
collection: form-input
key-takeaways:
  use-request-auto-complete:
    - <code>requestAutocomplete</code> 能够最大程度上简化结账的过程，同时能够提升用户体验。
    - 如果网页中<code>requestAutocomplete</code>可用，它会隐藏结账表单并直接引导用户来到确认页面。
    - 确保输入标签包含合适的自动完成属性。
remember:
  use-placeholders:
    - Placeholders disappear as soon as focus is placed in an element, thus
      they are not a replacement for labels.  They should be used as an aid
      to help guide users on the required format and content.
  recommend-input:
    - Auto-complete only works when the form method is post.
  use-datalist:
    - The <code>datalist</code> values are provided as suggestions, and users
      are not restricted to the suggestions provided.
  provide-real-time-validation:
    - Even with client-side input validation, it is always important to
      validate data on the server to ensure consistency and security in your data.
  show-all-errors:
    - You should show the user all of the issues on the form at once, rather than showing them one at a time.
  request-auto-complete-flow:
    - 如果你请求任何种类的私人信息或者信用卡数据，请确保网页处于SSL服务状态。否则，将会弹出对话框警告用户他们的信息可能会不安全。
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

  table.inputtypes th:nth-of-type(2) {
    min-width: 270px;
  }

  table.tc-heavyright th:first-of-type {
    width: 30%;
  }
</style>

{% include modules/takeaway.liquid list=page.key-takeaways.use-request-auto-complete %}

不像需要依靠特定支付手段的网站，`requestAutocomplete`需要从浏览器获得支付细目（比如姓名、收货地址以及信用卡信息），这些细目选择性地被浏览器保存，这和其他的自动完成表单非常相像。

### `requestAutocomplete`流程

最为理想的体验方式是网站会显示`requestAutocomplete`的对话框而不是读取展示结帐表单的页面。如果一切顺利，用户将不会看到表单。你可以很容易地为已存在的表单添加`requestAutoComplete`，这不需要改变任何字段名字。简单地给每一个表单元素添加`autocomplete`属性，并添加合适的值，以及在表单元素上添加`requestAutocomplete()`函数。浏览器会自动完成剩下的事。
<img src="imgs/rac_flow.png" class="center" alt="Request autocomplete flow">

{% include_code _code/rac.html rac javascript %}

`form`元素中的`requestAutocomplete`函数告诉浏览器应该填充表单。作为一个安全特性，这个函数必须通过用户手势，例如触摸或者鼠标单击来被唤醒。然后，弹出一个对话框询问用户许可以填充字段以及那些用户想要填充的详细内容。


{% include_code _code/rac.html handlerac javascript %}

当`requestAutocomplete`完成的时候，这个函数将会在表单成功填充成功时激活`autocomplete`事件。或者在表单不能够完成的情况下激活`autocompleteerror`事件。如果表单成功填充并且确认了你需要的，那就提交表单然后进行最终确认。

{% include modules/remember.liquid title="Remember" list=page.remember.request-auto-complete-flow %}

{% include modules/nextarticle.liquid %}

{% endwrap %}
