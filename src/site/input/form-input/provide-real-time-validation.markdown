---
layout: article
title: "提供实时验证"
description: "实时数据验证不仅保证你的数据条目清晰，同时也帮助提高用户体验。目前主流浏览器都拥有一些内置工具以帮助提供实时验证，并且可以防止用户提交无效信息。视觉提示应该被用来提示一个表单是被否准确地填写。"
introduction: "实时数据验证不仅保证你的数据条目清晰，同时也帮助提高用户体验。目前主流浏览器都拥有一些内置工具以帮助提供实时验证，并且可以防止用户提交无效信息。视觉提示应该被用来提示一个表单是否被准确地填写。"
article:
  written_on: 2014-04-30
  updated_on: 2014-04-30
  order: 3
rel:
  gplusauthor: https://plus.google.com/+PeteLePage
collection: form-input
key-takeaways:
  provide-real-time-validation:
    - 利用浏览器内置的实时验证属性，比如：
      <code>pattern</code>, <code>required</code>, <code>min</code>,
      <code>max</code>, 等等。
    - 使用JavaScript并且限制验证API满足更复杂的验证需求。
    - 实时显示验证错误，并且如果用户试图提交无效表单，那么就显示出用户需要修改的字段。
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
    - 即使有客户端的输入验证，验证服务器上的数据以确保你数据的统一性和安全性仍旧是非常重要的。
  show-all-errors:
    - 你应该向用户一次性展示表单的所有问题，而不是阶段性地展示。
  request-auto-complete-flow:
    - If you're asking for any kind of personal information or credit card
      data, ensure the page is served via SSL.  Otherwise the dialog will
      warn the user their information may not be secure.
---
{% wrap content %}

<style>
  table.inputtypes th:nth-of-type(2) {
    min-width: 270px;
  }

  table.tc-heavyright th:first-of-type {
    width: 30%;
  }
</style>

{% include modules/takeaway.liquid list=page.key-takeaways.provide-real-time-validation %}

### 使用这些属性以验证输入

#### `pattern` 属性

`pattern` 属性使用一个 [正则表达式（regular
expression）](http://en.wikipedia.org/wiki/Regular_expression)来验证输入字段 。例如：验证一个美国邮编（5个数字，有时可能由1个破折号再加上4个额外的数字），我们会像这样设置`pattern` :

{% highlight html %}
<input type="text" pattern="^\d{5,6}(?:[-\s]\d{4})?$" ...>
{% endhighlight %}

##### 常见正则表达式pattern（下表均为美国常用值）

<table class="table-2 tc-heavyright">
  <thead>
    <tr>
      <th data-th="Description">描述</th>
      <th data-th="Regular expression">正则表达式</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Description">通信地址</td>
      <td data-th="Regular expression"><code>[a-zA-Z\d\s\-\,\#\.\+]+</code></td>
    </tr>
    <tr>
      <td data-th="Description">邮编(美国)</td>
      <td data-th="Regular expression"><code>^\d{5,6}(?:[-\s]\d{4})?$</code></td>
    </tr>
    <tr>
      <td data-th="Description">IP地址</td>
      <td data-th="Regular expression"><code>^(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$</code></td>
    </tr>
    <tr>
      <td data-th="Description">信用卡号码</td>
      <td data-th="Regular expression"><code>^(?:4[0-9]{12}(?:[0-9]{3})?|5[1-5][0-9]{14}|3[47][0-9]{13}|3(?:0[0-5]|[68][0-9])[0-9]{11}|6(?:011|5[0-9]{2})[0-9]{12}|(?:2131|1800|35\d{3})\d{11})$</code></td>
    </tr>
    <tr>
      <td data-th="Description">社会安全保险号码</td>
      <td data-th="Regular expression"><code>^\d{3}-\d{2}-\d{4}$</code></td>
    </tr>
    <tr>
      <td data-th="Description">北美电话号码</td>
      <td data-th="Regular expression"><code>^(?:(?:\+?1\s*(?:[.-]\s*)?)?(?:\(\s*([2-9]1[02-9]|[2-9][02-8]1|[2-9][02-8][02-9])\s*\)|([2-9]1[02-9]|[2-9][02-8]1|[2-9][02-8][02-9]))\s*(?:[.-]\s*)?)?([2-9]1[02-9]|[2-9][02-9]1|[2-9][02-9]{2})\s*(?:[.-]\s*)?([0-9]{4})(?:\s*(?:#|x\.?|ext\.?|extension)\s*(\d+))?$</code></td>
    </tr>
  </tbody>
</table>

#### `required` 属性

如果`required`属性出现，那么该字段在提交前必须包含有效信息。例如：要求必须填写邮编地址，我们只要加上`required`属性:

{% highlight html %}
<input type="text" required pattern="^\d{5,6}(?:[-\s]\d{4})?$" ...>
{% endhighlight %}

#### `min`, `max` and `step` 属性

对于数值输入类型，像数字或范围，还有日期或时间输入，当滑动或缩放调整时，你可以定义它们的最小值和最大值。同样地，也能够定义他们步增/减量。例如：鞋子的尺寸应该设置最小值为1，最大值为13，步进值0.5。

{% highlight html %}
<input type="number" min="1" max="13" step="0.5" ...>
{% endhighlight %}

#### `maxlength` 属性

`maxlength` 属性能够被用来说明一个输入框或文本框的最多字符数。当你想限制用户提供的信息长度时，非常有用。例如：如果你想限制一个文件名在12个字符以内，你可以使用如下所示语句：
{% highlight html %}
<input type="text" id="83filename" maxlength="12" ...>
{% endhighlight %}

#### `novalidate` 属性

在某些情况下，你可能也会允许用户提交一个包含了无效输入的表单。可以给表单元素或特定输入字段添加novalidate属性。在这种情况下，如果表单验证了输入，所有的伪类和JavaScript APIs仍然会允许你检查。

{% highlight html %}
<form role="form" novalidate>
  <label for="inpEmail">Email address</label>
  <input type="email" ...>
</form>
{% endhighlight %}

{% include modules/remember.liquid title="Remember" list=page.remember.provide-real-time-validation %}

### 使用JavaScript以应对更复杂的实时验证

当内置验证加上正当表达式不够时，你可以使用[Constrains Validation API](http://dev.w3.org/html5/spec-preview/constraints.html#constraint-validation)，这是一款强有力的工具，用来管理自定义验证。API允许你设置一个自定义错误，检查元素是否有效，还有总结出一个元素无效的原因：

<table class="table-2 tc-heavyright">
  <thead>
    <tr>
      <th data-th="API">API</th>
      <th data-th="Description">描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="API"><code>setCustomValidity()</code></td>
      <td data-th="Description">设置自定义验证信息，并设置<code>ValidityState</code>对象的<code>customError</code>属性为<code>true</code>。</td>
    </tr>
    <tr>
      <td data-th="API"><code>validationMessage</code></td>
      <td data-th="Description">返回一个字符串，其内容为输入框没有通过验证的原因。</td>
    </tr>
    <tr>
      <td data-th="API"><code>checkValidity()</code></td>
      <td data-th="Description">如果元素满足其所有的限制条件则返回<code>true</code>，否则为<code>false</code>。</td>
    </tr>
    <tr>
      <td data-th="API"><code>validity</code></td>
      <td data-th="Description">返回<code>ValidityState</code>对象以显示元素的有效性状态。</td>
    </tr>
  </tbody>
</table>

#### 设置自定义验证信息

如果一个字段验证失败，那么使用`setCustomValidity()`来标记无效字段并且解释为什么该字段无效。例如：一个注册表单可能会要求用户输入两次邮箱地址以确认他们的邮件地址。在第二次输入使用一个模糊事件以验证两次输入和设置合适的网页回应。例如：

{% include_code _code/order.html customvalidation javascript %}

#### 防止提交无效表单

如果有无效数据，不是所有的浏览器都会禁止用户提交表单。你需要使捕获提交事件，并在表单元素上使用`checkValidity()`来判断表单是否有效，比如:

{% include_code _code/order.html preventsubmission javascript %}

### 显示实时反馈

在用户提交表单前否准确地完成了表单的情况下提供视觉提示是很有帮助的。HTML5同样地也提供了一系列的伪类，根据其值或属性来确定输入框样式。

<table class="table-2 tc-heavyright">
  <thead>
    <tr>
      <th data-th="Pseudo-class">伪类</th>
      <th data-th="Use">使用</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Pseudo-class"><code>:valid</code></td>
      <td data-th="Use">输入框中的值与验证要求相符的应用该样式。</td>
    </tr>
    <tr>
      <td data-th="Pseudo-class"><code>:invalid</code></td>
      <td data-th="Use">输入框中的值与验证要求不相符的应用该样式。</td>
    </tr>
    <tr>
      <td data-th="Pseudo-class"><code>:required</code></td>
      <td data-th="Use">输入框元素设有required属性的应用该样式。</td>
    </tr>
    <tr>
      <td data-th="Pseudo-class"><code>:optional</code></td>
      <td data-th="Use">输入框元素没设有required属性的应用该样式。</td>
    </tr>
    <tr>
      <td data-th="Pseudo-class"><code>:in-range</code></td>
      <td data-th="Use">数字输入框元素的值在其所设范围中的应用该样式。</td>
    </tr>
    <tr>
      <td data-th="Pseudo-class"><code>:out-of-range</code></td>
      <td data-th="Use">数字输入框元素的值不在其所设范围中的应用该样式。</td>
    </tr>
  </tbody>
</table>

当网页读取完毕，字段可能仍旧无效，甚至用户还没有机会输入的时候，验证也会立即开始。这同时也意味着，在用户输入的过程中，也会一直看到无效的样式。为了防止这种情况，你可以将CSS和JavaScript结合，只在用户访问过该字段之后显示无效样式。

{% include_code _code/order.html invalidstyle css %}
{% include_code _code/order.html initinputs javascript %}

{% include modules/remember.liquid title="Important" list=page.remember.show-all-errors %}

{% include modules/nextarticle.liquid %}

{% endwrap %}
